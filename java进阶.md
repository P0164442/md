# **1、springboot的数据库加密**
1. pom文件追加依赖

        <dependency>
        <groupId>com.github.ulisesbocchio</groupId>
        <artifactId>jasypt-spring-boot-starter</artifactId>
        <version>2.1.2</version>
        </dependency>

    <font color=red>注意：当前文档写的时候，用3.0的会报错误under 'spring.datasource.password' to java.lang.String用2.1.2版本正常。</font>

2. yml文件增加，robin@2020为加密秘钥，自己设置
   
        jasypt:
            encryptor:
                password: robin@2020
3. 链接字符串修改

        password: ENC(97FhhNi7U9dS8WX9lWlCieVCGtwkHTC5)

4. ENC内部的值是怎么来的
   
    4.1 首先我们cmd命令跳转到jar包所在路径

        E:\java\Repository\org\jasypt\jasypt\1.9.3
    4.2 然后执行以下命令

        java -cp jasypt-1.9.3.jar org.jasypt.intf.cli.JasyptPBEStringEncryptionCLI input="需要加密的字符串" password=加密秘钥（上面我们的是robin@2020） algorithm=PBEWithMD5AndDES