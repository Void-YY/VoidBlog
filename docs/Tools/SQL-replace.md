# 整合Log中的SQL

有时后端需要检查问题出在哪里，但是SQL语句的长度和参数数量非常大。
在日志文件中经常会以
- **SQL一行**Executing Statement:
```sql
    SELECT
        * 
    FROM
        e
        INNER JOIN f ON f.G = A.h 
    WHERE
        i = ? 
        AND j = ? 
        AND K <> ?
```
- **参数一行**Parameters: 
```json
[1, 2020-05-05 00:00:00.0, 2070-12-31 00:00:00.0, 9241, 527, 2, 009241, 527, 000527, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, , , , , , , , , , , , , , , , , , , , 0, 1, 6, 0211070, EMS720, 2020-02-27 10:08:44.067, 0211070, EMS720, 2020-02-27 13:38:22.547, null, null, null, 98002009.0211070, EMS720, 2020-02-27 13:34:19.342]
```
的形式出现。  
此时需要手动将SQL中的参数“?”按顺序替换为数组中的参数。这个过程十分浪费时间精力也容易出错。
所以我想用浏览器的控制台随手处理掉这个繁琐的过程。
以下为JavaScript的实现方式  
```javascript
    /**
    * 整合Log中的SQL
    * @param str SQL
    * @param arr 参数
    */
    function repleceSql(str,arr){
        var i = 0;
        console.warn(str.replace(/\?/g,function(){return arr[i++]}));
    }
```

