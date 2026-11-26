# 跨插件宿主的请求扩展链与候选版本切换

扩展用 plugin/rev 区分，schema 引用本地合成内容类型。host 为实际仿真执行端。

## 输入消息

每个 JSONL 文件是一段独立输入记录。每行包括整数秒点 at、消息名称 kind 与对象 record。at 代表中间件收到消息的时刻，同一秒内按行序提交。业务时间和对应回执引用写在 record 中。

- **plugin**　plugin、rev、input、output、tenants、required 以及 hosts。
- **publish**　chain、revision、members 及 tenants。members 给出顺序中的 plugin@rev。
- **invoke**　tenant 为 call 提交 chain 和 input。
- **claim**　host 领取 call 内明示段，attempt 给接单身份。
- **settle**　host 对 attempt 回报 done、never_started 或 unknown。done 含 output。
- **withdraw**　主管撤回 plugin/rev 新受理资格。
- **restart**　服务重启。
- **inspect**　按 call 查询。

## 示例

- mixed-chain-is-not-a-release.jsonl
- revocation-after-claim.jsonl

## 读取演练资料

运行环境为 Python 3.11，只使用标准库。在根目录运行以下命令可核对文件格式及读取接口。

    python3 scenario_reader.py examples
    python3 -m unittest discover -v

资料读取器会按文件统计各类参与方消息的数量。演练记录只描述各参与方交给系统的输入。
