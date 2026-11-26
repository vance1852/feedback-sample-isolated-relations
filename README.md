# 关系数据沙箱的可复现裁剪与撤回

本项目的本地资料保存各方使用的原始关系、接入操作及外部观察。记录里的人员、机构和凭据均为合成。各资料目录独立使用。

## 原始关系及接入约定

households 以跨表 household_id 关联，person_id、contact_id、receipt_id 为不同身份域。允许包含的跨范围关系由 policy_edges 定义。mode=proxy 表示只允许输出保持引用可用的替身及列清单，mode=block 表示不能将该关系参与的记录输出为可用组合。用途样本库通过受管理入口访问。stable_class 定义同用途兼容批次的映射域，不同用途独立。
applications 记录批准范围及保留期。撤回的范围以 identity_kind 与 id 声明，批准目的或原断面不能取消撤回事后新增的限制。timeline 的 read 与 continue 表示真实消费者用一个已知批次查询。

catalog.json 列出各数据库文件的角色、源表及运行限额。timeline.json 中的 at 是操作或观察到达的顺序，同一时刻按数组内的位置排列。args 是相应动作携带的字段，各记录里的其他位点使用该记录的业务含义。SQL 文件中只有开始联调前的行和表定义。

## 装载资料

Python 3.11.2 自带 SQLite 3.40.1。以下命令从合成资料生成独立数据库，目标文件须尚未存在。可用 sqlite3 打开生成的文件并读取源表。

~~~sh
python3 load_sample.py fixtures/shared-contact-revoked-after-approval main-local.db --role main
~~~

执行 python3 -m unittest -v 可核对各份资料的 SQL 是否可装载、基础表行数，以及普通 SQLite 读写连接能否并行打开。
