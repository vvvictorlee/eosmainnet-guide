#当我们修改合约或者宪法的时候需要2/3 个生产者确认后既可修改，这时候我们就需要进行多重签名来操作。

#multisig总共有6个action，如下：
```
root@ip-172-30-101-34:~/EOS-Jungle-Testnet# ./cleos.sh multisig 
ERROR: RequiredError: Subcommand required
Multisig contract commands
Usage: /root/eos/build/programs/cleos/cleos multisig SUBCOMMAND

Subcommands:
  propose                     Propose transaction
  review                      Review transaction
  approve                     Approve proposed transaction
  unapprove                   Unapprove proposed transaction
  cancel                      Cancel proposed transaction
  exec                        Execute proposed transaction
```

***

如果我们想提议意见事，主要有以下几个步骤：

**1.我们能够通过命令查到当前有多少生产者可以支持**
```
$./cleos.sh multisig propose transfer [{"actor": "eosio.prods", "permission": "active"}] [{"actor": "eosio", "permission": "active"}] eosio.token transfer '{"from": "eosio", "to": "eosstore4321", "quantity": "123.0000 EOS", "memo": "test"}'  eosstore4321 24 -p eosstore4321
```
```
Usage: /root/eos/build/programs/cleos/cleos multisig propose [OPTIONS] proposal_name requested_permissions trx_permissions contract action data [proposer] [proposal_expiration]

Positionals:
  proposal_name TEXT          proposal name (string) (required)
  requested_permissions TEXT  The JSON string or filename defining requested permissions (required)
  trx_permissions TEXT        The JSON string or filename defining transaction permissions (required)
  contract TEXT               contract to wich deferred transaction should be delivered (required)
  action TEXT                 action of deferred transaction (required)
  data TEXT                   The JSON string or filename defining the action to propose (required)
  proposer TEXT               Account proposing the transaction
  proposal_expiration         Proposal expiration interval in hours
```
**2.申请提议**
```
 $./cleos.sh multisig propose transfer [{"actor": "eosio.prods", "permission": "active"}] [{"actor": "eosio", "permission": "active"}] eosio.token transfer '{"from": "eosio", "to": "eosstore4321", "quantity": "123.0000 EOS", "memo": "test"}'  eosstore4321 34 -p eosstore4321
```
```
Usage: /root/eos/build/programs/cleos/cleos multisig propose [OPTIONS] proposal_name requested_permissions trx_permissions contract action data [proposer] [proposal_expiration]

Positionals:
  proposal_name TEXT          proposal name (string) (required)
  requested_permissions TEXT  The JSON string or filename defining requested permissions (required)
  trx_permissions TEXT        The JSON string or filename defining transaction permissions (required)
  contract TEXT               contract to wich deferred transaction should be delivered (required)
  action TEXT                 action of deferred transaction (required)
  data TEXT                   The JSON string or filename defining the action to propose (required)
  proposer TEXT               Account proposing the transaction
  proposal_expiration         Proposal expiration interval in hours
```

**3.然后如果支持提议就可以执行下面的命令。**
```
$./cleos.sh multisig approve eosstore4321 prop111 '{"actor": "eosstore4321", "permission": "active"}' -p eossotre4321
```
```
root@ip-172-30-101-34:~/EOS-Jungle-Testnet# ./cleos.sh multisig approve 
ERROR: RequiredError: proposer
Approve proposed transaction
Usage: /root/eos/build/programs/cleos/cleos multisig approve [OPTIONS] proposer proposal_name permissions

Positionals:
  proposer TEXT               proposer name (string) (required)
  proposal_name TEXT          proposal name (string) (required)
  permissions TEXT            The JSON string of filename defining approving permissions (required)

Options:
  -h,--help                   Print this help message and exit
  -x,--expiration             set the time in seconds before a transaction expires, defaults to 30s
  -f,--force-unique           force the transaction to be unique. this will consume extra bandwidth and remove any protections against accidently issuing the same transaction multiple times
  -s,--skip-sign              Specify if unlocked wallet keys should be used to sign transaction
  -j,--json                   print result as json
  -d,--dont-broadcast         don't broadcast transaction to the network (just print to stdout)
  -r,--ref-block TEXT         set the reference block num or block id used for TAPOS (Transaction as Proof-of-Stake)
  -p,--permission TEXT ...    An account and permission level to authorize, as in 'account@permission'
  --max-cpu-usage-ms UINT     set an upper limit on the milliseconds of cpu usage budget, for the execution of the transaction (defaults to 0 which means no limit)
  --max-net-usage UINT        set an upper limit on the net usage budget, in bytes, for the transaction (defaults to 0 which means no limit)
```