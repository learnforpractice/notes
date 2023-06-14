以下以4个节点为例讲解如何启动一个本地测试的tip网络：

先用下面的方式安装tip:

```
git clone https://github.com/MixinNetwork/tip
cd tip
go install
```

```bash
./tip key
```
生成如下的密码对：

```
2d30693fc291d3ee14ce03a4b74de58a147ccc014bb42575ae8441a49304d821
5JHMWABjaYUSirUg6TVM9rm2k5nanXHW7PCZRivfrjA4vFfHDZ1Xtav6E9TpfCNNRvZxF2up9goQnCG7nxri6Vt2C6LhtTdCkYwpGxayyT4kKo2doj6N7yGohGFzDLLy1CB3dstmUvdUNkGdyG6SbT9G8VkhU5V4B4V7yqxii7cKgWXheZotrx
```

`2d30693fc291d3ee14ce03a4b74de58a147ccc014bb42575ae8441a49304d821`为私钥，请妥善保管，不能公开。
`5JHMWABjaYUSirUg6TVM9rm2k5nanXHW7PCZRivfrjA4vFfHDZ1Xtav6E9TpfCNNRvZxF2up9goQnCG7nxri6Vt2C6LhtTdCkYwpGxayyT4kKo2doj6N7yGohGFzDLLy1CB3dstmUvdUNkGdyG6SbT9G8VkhU5V4B4V7yqxii7cKgWXheZotrx`为公钥，会被公开

再执行三次生成如下的四组密码信息：

```
2d30693fc291d3ee14ce03a4b74de58a147ccc014bb42575ae8441a49304d821
5JHMWABjaYUSirUg6TVM9rm2k5nanXHW7PCZRivfrjA4vFfHDZ1Xtav6E9TpfCNNRvZxF2up9goQnCG7nxri6Vt2C6LhtTdCkYwpGxayyT4kKo2doj6N7yGohGFzDLLy1CB3dstmUvdUNkGdyG6SbT9G8VkhU5V4B4V7yqxii7cKgWXheZotrx

07613f38e6b4990830adeeef027c6fcdb536a41d705141a72d472821efd3d8c6
5HZZR9PMsfabsZG57WDAEdaYRAFb7qLYMc4y5xjR8rgYxLBa4cwV1M4NH5JFxrBKvtmQMSCLwgzAemLNmPxXM9oi5vt9ft3VhUugjvBjEhC2qUQzUMxKAzFgxfSze9ZoALYWbUM98S4A2JEqCp9f3QgmG2J6TbFRvDF5pAip61Pxh9vjM9dFP3

35e294315f39a4bead3f8d9a4e28f4ddaa2567450bb1ef3c6f8a862717e74b45
5JCdHnyico8wc3a1YKq22GEJXbXfq7GzENh6Xo4AfWMbiguGeHjMHUabuyr9bzpRsycdtf1LJFnxmndn5qd3LtZeWF5tugJjmgBdk7i4Jj3Laeq7FukPtadrzu5BGGb9NqH2HSbFYW22eqzjB9CFTXuwhDejT86k3E2KaVxT26dHRyKiXxjzDZ

770e5d674c062b058c52147fa1cf1c9404d44ff12089d6b6020c1ba14aabce0d
5HvUui1sP3n4ySMGUNXsjQgv6DJU6CFhFZZeRPtoxMEJmj7Lvqzv2n8cHhu9tcoYoQNPWCjP8V6B9A9XhNGWD1NBiXDoxCewgziJYMynNMXFsPH6tjbHUN2y8Y2DWWZ8LRUYN9PaShnxYLfiNXW1GvfpYgZ5EobweKVugdSitmSDz8G2m3bB9R
```

获取机器人的配置信息：

* 从网址`https://developers.mixin.one/dashboard`选择一个机器人，再选择`Secret`选项卡，在`APP SESSION`点`Ed25519 session`生成一个json格式的配置。

如下所示：

```json
{
  "pin": "****",
  "client_id": "3e72c******d375e7",
  "session_id": "e0165********81da0d",
  "pin_token": "5QKS**********qK4RhY",
  "private_key": "DHM36cxkdg-4**********xqT5KT2hcGJg"
}
```

如果是本地测试，一共需要四个机器人的配置信息，不够可以在 dashboard 点`New App`进行创建。

根据上面的信息配置 toml 格式的配置文件，如下为某个节点的示例：

```toml
[api]
port = 7001

[store]
dir = "/tmp/tip1"

[messenger]
user = "3e72ca0c-********-4d8471d375e7"
session = "e01654********581da0d"
key = "DHM36cxkdg-4C6z********sZ9I0gxqT5KT2hcGJg"
buffer = 64
conversation = "dfe49fde******3f93f1859b3"

[node]
key = "2d30693fc291d3ee14ce03a4b74de58a147ccc014bb42575ae8441a49304d821"
signers = [
    "5JHMWABjaYUSirUg6TVM9rm2k5nanXHW7PCZRivfrjA4vFfHDZ1Xtav6E9TpfCNNRvZxF2up9goQnCG7nxri6Vt2C6LhtTdCkYwpGxayyT4kKo2doj6N7yGohGFzDLLy1CB3dstmUvdUNkGdyG6SbT9G8VkhU5V4B4V7yqxii7cKgWXheZotrx",
    "5HZZR9PMsfabsZG57WDAEdaYRAFb7qLYMc4y5xjR8rgYxLBa4cwV1M4NH5JFxrBKvtmQMSCLwgzAemLNmPxXM9oi5vt9ft3VhUugjvBjEhC2qUQzUMxKAzFgxfSze9ZoALYWbUM98S4A2JEqCp9f3QgmG2J6TbFRvDF5pAip61Pxh9vjM9dFP3",
    "5JCdHnyico8wc3a1YKq22GEJXbXfq7GzENh6Xo4AfWMbiguGeHjMHUabuyr9bzpRsycdtf1LJFnxmndn5qd3LtZeWF5tugJjmgBdk7i4Jj3Laeq7FukPtadrzu5BGGb9NqH2HSbFYW22eqzjB9CFTXuwhDejT86k3E2KaVxT26dHRyKiXxjzDZ",
    "5HvUui1sP3n4ySMGUNXsjQgv6DJU6CFhFZZeRPtoxMEJmj7Lvqzv2n8cHhu9tcoYoQNPWCjP8V6B9A9XhNGWD1NBiXDoxCewgziJYMynNMXFsPH6tjbHUN2y8Y2DWWZ8LRUYN9PaShnxYLfiNXW1GvfpYgZ5EobweKVugdSitmSDz8G2m3bB9R"
]
timeout = 10
```

user = "3e72ca0c-********-4d8471d375e7"
session = "e01654********581da0d"
key = "DHM36cxkdg-4C6z********sZ9I0gxqT5KT2hcGJg"
buffer = 64
conversation = "dfe49fde******3f93f1859b3"

`messenger`中的各配置项和机器人中的json格式的对应关系如下：

`user` : `client_id`，
`session` : `session_id`
`key` : `private_key`

接下来讲下如何获取 conversation

用手机上的mixin messenger创建一个群组，把四个机器人都拉到群组中。

把链接`https://mixin.one/context`发到群组中，打开这个链接，显示的uuid即为coversaion


配置文件写好后，假设配置文件分别为tip1.toml, tip2.toml, tip3.toml, tip4.toml。 分别运行下面四条命令启动signer：

```bash
./tip -c tip1.toml signer
./tip -c tip2.toml signer
./tip -c tip3.toml signer
./tip -c tip4.toml signer
```

再分别执行下面的四条命令：

```bash
./tip -c tip4.toml setup --nonce 1000000000
./tip -c tip4.toml setup --nonce 1000000000
./tip -c tip4.toml setup --nonce 1000000000
./tip -c tip4.toml setup --nonce 1000000000
```

可以看到各个signer有如下的输出：

```
2023/06/09 12:28:27 Poly public: 12d0884d84e37e5159873884b664bcc17ed06099ab36d002cf62ef4c81ce30c4587c1d186bff5db910143a8f30ac49434a41772d73c33868f0af9f528ee99a4742a4114cb0e3ec517372d329bb0190c658ae7623eb9e33aa7e6af0a0b4e836e70af288c4c9129c3e05e4f4e07fff565a08c16b29b084be2c1ecf8f83e0d51a643a59c654bd2e7b1b2c81f237eb3907f6dffbbe43c67788d5abe06219615795d0006af80c628088cbf6d1d45cabb7d0568be1b74e941128984e5971d6bde96cc47974fcd81a2555480f3c8a0e5da9f732c2cc9dcd59b130383436d57bd37ea3474d7354ed411d57459be717e15ee2bf3b4f233ddf277ae7e4a6d3cb3eb4deb38a6c11f708bb91c46296730855f6647cebe172305b06ba98f963f3147d3ab0f6f9651171141c46c6abb24c58a63c93b5f08c994af84d2550e7b48a8196b4378ee665ccdcf76235161f606ebd9af41e08a0affc288a0d402c3de8baed2ab35f2fea84446eef6435d60e54921f9cd97875406887f4b712e22e665ec0830408ba39b3
2023/06/09 12:28:27 

Poly share: 0000000124616ac38d985611df3526d228fa5eec997153ab1d82641478e893db5f1d4313
```


其中的Poly public为公钥信息的信息
Poly share为用于多签的私钥信息，不能公开


接下来开启api接点：
```bash
./tip -c tip1.toml api
./tip -c tip2.toml api
./tip -c tip3.toml api
./tip -c tip4.toml api
```

配置nodes.json，在sign命令中会用到。下面是一个例子：

```json
{
    "commitments":[
        "5HUyt8JzKFnZ5UHuY4VRt1zWa3g5EZxE2nUYDnbnQUXpJXLqfTuKg9mAqvdaxiRaGMQdz3s9RZfqSGdCHKobYmtManpfFxntt4Uk5MnxXBZdV5p9TfAKXnqAEueDz13RFtks35zYDrhcWDUKC6UWCfsdpE94Vsm1mkMQPoAp5njY5vwtKYyeoN",
        "5HwTNSKJ7mdChQUtjxuTmfq6N6tKLmQHPU2hmHw8Zwf5ganM3Phv8furGBAZuMpsbDJ2SERuk8RQyiyPRcph9ZAVK6mGuu22gYuKtkBXFGUvc7YwviQCV5tWPoXoNTqCLXt4ki5EnSqkBGNziTmvZY7HWqkuJevag2d1YpC9jXCEhKL9kkisjU",
        "5JWkMfkb2qkeynCdMvmLsdGAZqRGQpXtqJDLrntajNKkA3PKmCpREi53zHTAWejggppexahF9mDjnapspJjg1Rp9aQmPothUCGT1nSQ7hGn2GwusEStsQKEMvZRerPc7Xv1AvnQb18WdNWUMndMRsVBp3bJvW3LRoJLWrpvBYMCWJQ4fS2irkS"
    ],
    "signers":[
        {
            "identity":"5HY3MuTUPkkKJYmRskRWRjYxpsKscJ3R7xetTYfXUEQAxL9C2TBKWAKzvF7DjDRu8iYFtA6xLcHouaZHJXRbFfD8eru6JTTk8AL92FesmkUTxJyjsFjfmjr7BjqQhiUAoAP9rNdC8MaT7hwnsNvYsZE3BW3rnTozpJkgnNXifFQ1QdK2bYJzFo",
            "api":"http://127.0.0.1:7001"
        },
        {
            "identity":"5HrMVdZbqRHGKKTZbtXcKQ6NZZaDneNxFNh4yxvogksb33YypnHujyThfFwdofFscJyAkhgxwaXcjtiw72uLytrqXffmCLQY7E8Fy1mdLesb2xj1p3DxdCX47vNnVwxWWd6hDCCnqTSVPj8aDgBK7nBdgRS1K7DPLHLU5trjXrFhQKhopNQaYL",
            "api":"http://127.0.0.1:7003"
        },
        {
            "identity":"5HvxiD7CNpTBHfzpd6tYTzsC9rfQQTCRXn4P9R664UZJVFxxUGnHaL9CPBqiKJMwuZu8F6WSaRYctb1bw6kpuDpMih6zPRvkD6joEGMdMK36sVF3xtwogZVqgTZ5N467exuCF6EvEveFbyBuwx1f8U1F2Asai3jzmQoeaizhDedDhovRfkuBJ9",
            "api":"http://127.0.0.1:7002"
        },
        {
            "identity":"5Jcc3ixKVjxa6KJVLi15WGEL22HbhPoBfLf2HMPG11eXUSrwC5Eynuk77q2Fi2JuNzyxtvaroQxaN4FRS2FuMk1JkshHDHX57oNDcn51AAWHFPuRNH3QNMfqFGgiSnLZPHoxNcgiRkJWaiHNtpF4VaoSjJkxqd4w9SVMABJAeMLW8oRofLsCkc",
            "api":"http://127.0.0.1:7004"
        }
    ]
}
```

其中的`commitments`可以通过下面的命令获取：
```bash
curl http://127.0.0.1:7004
```

请求签名：

```bash
./tip sign --config nodes.json --key 7aec354ba7cb5be253c511c50e244cf94aa4a70e3700dacc5d772516326cbebc --ephemeral 8aec354ba7cb5be253c511c50e244cf94aa4a70e3700dacc5d772516326cbebb --nonce 112235 --watcher 9aec354ba7cb5be253c511c50e244cf94aa4a70e3700dacc5d772516326cbebb
```

nonce在每次请求时必须比前一次的请求大
