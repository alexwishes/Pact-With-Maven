# 说明
网上的代码都是基于gradle的, 由于团队需要使用maven, 特将代码转换成maven, 供有需要的人使用~~~
原来的gradle版本代码:https://github.com/Mikuu/Pact-JVM-Example
## 一. 结构介绍
1. consumer里面包含2个文件夹, 代表2个consumer: miku和nanoha
2. provider就是服务提供者

## 二. 使用方法
### 消费者端
1-1. 生成miku的契约文件
```
cd consumer/miku
mvn clean test
```
1-2. 将契约文件推送到broker中
```
mvn pact:publish
```
2-1. 生成nanoha的契约文件
```
cd consumer/nanoha
mvn clean test
```
2-2. 将契约文件推送到broker中
```
mvn pact:publish
```
### 提供者端
验证契约是否正确
```
cd provider
mvn pact:verify
# 验证并把结果推送到broker页面
# mvn pact:verify -Dpact.verifier.publishResults=true
```
## 三. Broker信息配置
### 1. Provider配置
Provider的pom.xml文件中以下部分, 按需更改为自己的broker地址
```
<plugin>
    <groupId>au.com.dius.pact.provider</groupId>
    <artifactId>maven</artifactId>
    <version>${pact.version}</version>
    <configuration>
        <serviceProviders>
            <serviceProvider>
                <name>ExampleProvider</name>
                <pactBrokerUrl>http://192.168.121.221:9292/</pactBrokerUrl>
            </serviceProvider>
        </serviceProviders>
    </configuration>
</plugin>
``` 
### 2. Consumer配置
```
<plugin>
    <groupId>au.com.dius.pact.provider</groupId>
    <artifactId>maven</artifactId>
    <version>4.1.6</version>
    <configuration>
        <pactDirectory>target/pacts</pactDirectory> 
        <pactBrokerUrl>http://192.168.121.221:9292</pactBrokerUrl>
        <projectVersion>1.0.100</projectVersion> 
        <trimSnapshot>true</trimSnapshot>
        <skipPactPublish>false</skipPactPublish>
    </configuration>
</plugin>
```
