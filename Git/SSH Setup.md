**Starting Service**
`start-ssh-agent.cmd`

#### SSH key authentication
Open GitBash
1. Generate the key, preferebly using your email as an Id
`$ ssh-keygen -t rsa -b 4096 -C "username@domain.com"
2. Execute shell command
`$ eval $(ssh-agent -s)`
3. Add the key to the user's home directory 
`$ ssh-add ~/.ssh/id_rsa`
4. Copy they key contents to clipboard.
`$ clip < ~/.ssh/id_rsa.pub`
5. In Github.com, go to: Profile > Settings > SSH and GPG keys, create New SSH Key: Title: Computer ID, Key & paste from clipboard

Cloning the repo
GitBash:
`$ ssh -T git@github.com`
`$ git clone git@github.com:Game-Studio/Project-Name.git`


