# java常用的工具类

## 文件操作
### org.apache.commons.io.FileUtils
1. deleteDirectory 删除文件夹
2. readFileToString 以字符形式读取文件内容。
3. deleteQueitly 删除文件或文件夹且不会抛出异常。
4. copyFile 复制文件
5. writeStringToFile 把字符写到目标文件，如果文件不存在，则创建。
6. forceMkdir 强制创建文件夹，如果该文件夹父级目录不存在，则创建父级。
7. write 把字符写到指定文件中
8. listFiles 列举某个目录下的文件(根据过滤器)
9. copyDirectory 复制文件夹
10. forceDelete 强制删除

### org.apache.commons.io.FilenameUtils
getExtension 返回文件后缀名
getBaseName 返回文件名，不包含后缀名
getName 返回文件全名
concat 按命令行风格组合文件路径(详见方法注释)
removeExtension 删除后缀名
normalize 使路径正常化
wildcardMatch 匹配通配符
seperatorToUnix 路径分隔符改成unix系统格式的，即/
getFullPath 获取文件路径，不包括文件名
isExtension 检查文件后缀名是不是传入参数(List<String>)中的一个

## 字符串操作
### org.apache.commons.lang3.StringUtils
1.isBlank 字符串是否为空 (trim后判断)
2. isEmpty 字符串是否为空 (不trim并判断)
3. equals 字符串是否相等
4. join 合并数组为单一字符串，可传分隔符
5. split 分割字符串
6. EMPTY 空字符串
7. replace 替换字符串
8. capitalize 首字符大写

### org.springframework.util.StringUtils
1. hasText 检查字符串中是否包含文本
2. hasLength 检测字符串是否长度大于0
3. isEmpty 检测字符串是否为空（若传入为对象，则判断对象是否为null）
4. commaDelimitedStringToArray 逗号分隔的String转换为数组
5. collectionToDelimitedString 把集合转为CSV格式字符串
6. replace 替换字符串
7. delimitedListToStringArray 相当于split
8. uncapitalize 首字母小写
9. collectionToDelimitedCommaString 把集合转为CSV格式字符串
10. tokenizeToStringArray 和split基本一样，但能自动去掉空白的单词

## 集合操作

### org.apache.commons.collections.CollectionUtils
1. isEmpty 是否为空
1. select 根据条件筛选集合元素
1. transform 根据指定方法处理集合元素，类似List的map()。
1. filter 过滤元素，雷瑟List的filter()
1. find 基本和select一样
1. collect 和transform 差不多一样，但是返回新数组
1. forAllDo 调用每个元素的指定方法。
1. isEqualCollection 判断两个集合是否一致

### org.apache.commons.lang3.ArrayUtils
1. contains 是否包含某字符串
1. addAll 添加所有
1. clone 克隆一个数组
1. isEmpty 是否空数组
1. add 向数组添加元素
1. subarray 截取数组
1. indexOf 查找下标
1. isEquals 比较数组是否相等
1. toObject 基础类型数据数组转换为对应的Object数组

## bean操作
### org.apache.commons.beanutils.PropertyUtils
1. getProperty 获取对象属性值
1. setProperty 设置对象属性值
1. getPropertyDiscriptor 获取属性描述器
1. isReadable 检查属性是否可访问
1. copyProperties 复制属性值，从一个对象到另一个对象
1. getPropertyDiscriptors 获取所有属性描述器
1. isWriteable 检查属性是否可写
1. getPropertyType 获取对象属性类型

### org.apache.commons.beanutils.BeanUtils
1. copyPeoperties 复制属性值，从一个对象到另一个对象
1. getProperty 获取对象属性值
1. setProperty 设置对象属性值
1. populate 根据Map给属性复制
1. copyPeoperty 复制单个值，从一个对象到另一个对象。
1. cloneBean 克隆

## 加密
### org.apache.commons.codec.digest.DigestUtils
1. md5Hex MD5加密，返回32位
1. sha1Hex SHA-1加密
1. sha256Hex SHA-256加密
1. sha512Hex SHA-512加密
1. md5 MD5加密，返回16位
