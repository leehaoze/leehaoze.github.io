---
title: Spring Boot实现文件上传与下载
date: 2019-08-15 11:20:09
tags:
- Spring
- Java
---
参考自[掘金的文章](https://juejin.im/post/5c9e57e2f265da307a160328)

# 设置配置文件
在Spring的配置文件`application.properties`中新增文件上传相关的配置
```
## MULTIPART (MultipartProperties)
# 开启 multipart 上传功能
spring.servlet.multipart.enabled=true
# 文件写入磁盘的阈值
spring.servlet.multipart.file-size-threshold=2KB
# 最大文件大小
spring.servlet.multipart.max-file-size=200MB
# 最大请求大小
spring.servlet.multipart.max-request-size=215MB

## 文件存储所需参数(此为自定义参数 需要单独进行配置)
# 所有通过 REST APIs 上传的文件都将存储在此目录下
file.upload-dir=./uploads
```

# 为自定义的配置参数创建POJO类
- 引入自定义配置依赖
```
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
```
- 创建配置类
```
@ConfigurationProperties(prefix = "file")
@Getter
@Setter
public class FileProperties {
    /**
     * 上传目录
     */
    private String uploadDir;
}
```
- 在应用入口添加注解开启自定义配置
```
@EnableConfigurationProperties({
        FileProperties.class
})
public class MainApplication {
    public static void main(String[] args) {
        SpringApplication.run(RpaMainApplication.class, args);
    }
}
```
- 使用 通过注解直接注入到构造函数中
```
    /**
     * 文件在本地存储的地址
     */
    private final Path fileStorageLocation;
    
    @Autowired
    public ApplicationServiceImpl(FileProperties fileProperties) {
        this.fileStorageLocation = Paths.get(fileProperties.getUploadDir()).toAbsolutePath().normalize();
        try {
            Files.createDirectories(this.fileStorageLocation);
        } catch (Exception ex) {
            throw new Exception(StatusCode.FILE_UPLOAD_FAILED);
        }
    }
```

# 存储文件函数
```
    /**
     * 存储文件到系统
     *
     * @param file 文件
     * @return 文件名
     */
    private void storeFile(MultipartFile file, String fileName) {
        try {
            // Copy file to the target location (Replacing existing file with the same name)
            Path targetLocation = this.fileStorageLocation.resolve(fileName);
            Files.copy(file.getInputStream(), targetLocation, StandardCopyOption.REPLACE_EXISTING);
        } catch (IOException ex) {
            throw new Exception(StatusCode.FILE_UPLOAD_FAILED);
        }
    }
```

# 加载文件函数
```
/**
     * 加载文件
     * @param fileName 文件名
     * @return 文件
     */
    private Resource loadFileAsResource(String fileName) {
        try {
            Path filePath = this.fileStorageLocation.resolve(fileName).normalize();
            Resource resource = new UrlResource(filePath.toUri());
            if(resource.exists()) {
                return resource;
            } else {
                throw new Exception(RpaStatusCode.FILE_NOT_FOUND);
            }
        } catch (MalformedURLException ex) {
            throw new Exception(RpaStatusCode.FILE_NOT_FOUND);
        }
    }
```

# 上传文件路由实现
- Controller 参数使用一个bean
```
    @PostMapping(value = "/add")
    public ResultDto addApplication(HttpServletRequest request, ApplicationAddForm applicationAddForm) {
        //todo 参数校验
        applicationService.addApp(applicationAddForm);
        return new ResultDto(RpaStatusCode.SUCCESS);
    }
```
- bean内具体属性设置一个MultipartFile
```
@Getter
@Setter
@AllArgsConstructor
//todo 添加参数的校验
public class ApplicationAddForm {
    private String title;

    private String avatar;

    @JsonProperty(value = "version_num")
    private Integer versionNum;

    private Integer type;

    private Integer developer;

    private String description;

    private MultipartFile blFile;
```

这样便可以接受文件和表单一起提交过来的请求了，直接在Service中取出实体中的blFile，调用`StoreFile`方法储存

``` 
        String storeFileName = "Test"
        MultipartFile blFile = applicationAddForm.getBlFile();
        storeFile(blFile, storeFileName);

```

# 下载文件路由实现
```
  @GetMapping(value = "/download")
    public ResponseEntity<Resource> downApplication(HttpServletRequest request, @RequestParam(value = "filename") String filename, ) {
        Resource resource = applicationService.loadFileAsResource(filename);

        String contentType;
        try {
            contentType = request.getServletContext().getMimeType(resource.getFile().getAbsolutePath());
        } catch (IOException ex) {
            contentType = "application/octet-stream";
        }

        return ResponseEntity.ok()
                .contentType(MediaType.parseMediaType(contentType))
                .header(HttpHeaders.CONTENT_DISPOSITION, "attachment; filename=\"" + resource.getFilename() + "\"")
                .body(resource);

    }
```
