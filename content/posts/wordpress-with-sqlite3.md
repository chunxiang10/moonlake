# wordpress with sqlite3

1. 将目录下的wp-config-sample.php复制粘贴一份重命名为wp-config.php
2. 打开wp-config.php修改以下配置
   ```php
   // ** MySQL 设置 - 具体信息来自您正在使用的主机 ** //
   /** WordPress数据库的名称 */
   define('DB_NAME', 'wordpress');//wordpress是数据库名,可以自定义
   /** MySQL数据库用户名 */
   define('DB_USER', '');
   /** MySQL数据库密码 */
   define('DB_PASSWORD', '');
   /** MySQL主机 */
   define('DB_HOST', 'localhost');
   /** 创建数据表时默认的文字编码 */
   define('DB_CHARSET', 'utf8');
   /** 数据库整理类型。如不确定请勿更改 */
   define('DB_COLLATE', '');
   //define('WP_ALLOW_REPAIR', true);//数据库修复时使用
   define('DB_TYPE', 'sqlite');    //mysql or sqlite`
   ```
3. 将 `db.php`复制到到wordpress安装目录下的wp-content目录中
4. 访问博客地址
