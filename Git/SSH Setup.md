**Starting Service**
`start-ssh-agent.cmd`

#### SSH key authentication
Open GitBash
1. Generate the key, preferebly using your email as an Id
`$ ssh-keygen -t rsa -b 4096 -C "username@domain.com"
2. When prompted to 'Enter file in which to save the key' **Leave it BLANK** and Press Enter
3. Execute shell command
`$ eval $(ssh-agent -s)`
4. Add the key to the user's home directory 
`$ ssh-add ~/.ssh/id_rsa`
5. Copy they key contents to clipboard.
`$ clip < ~/.ssh/id_rsa.pub`
6. In Github.com, go to: Profile > Settings > SSH and GPG keys, create New SSH Key: Title: Computer ID, Key & paste from clipboard

Cloning the repo
GitBash:
`$ ssh -T git@github.com`
`$ git clone git@github.com:Game-Studio/Project-Name.git`



#### Troubleshooting
##### To see what keys are added, run: `$ ssh-add -l -E sha256`
##### 'WARNING: UNPROTECTED KEY FILE' Error
<details>
<summary> Step by step with images </summary>
I'm not super sure what causes this error, but here's how to fix just in case
![image](https://github.com/Born-Studios/knowledge-Base/assets/47042497/673a414e-94ed-47cf-877e-9ecec00d159d)

1. Right-click your private key and select properties 
2. Go to Security tab and click 'advanced'  
<img src="https://github.com/Born-Studios/knowledge-Base/assets/47042497/364307f1-5fb9-4193-a29b-aff8d301bf52" width="50%">  

3. Click 'Disable Inheritance'  
<img src="https://github.com/Born-Studios/knowledge-Base/assets/47042497/4abae9f4-a801-4156-9244-2bde30213ee1" width="50%">  

4. Click 'Convert inherited permissions into explicit permissions on this object.  
<img src="https://github.com/Born-Studios/knowledge-Base/assets/47042497/cec3cce1-e555-4f51-a48f-4faa070ef981" width="50%">  

5. Click 'Add' then on the next window 'Select a Principal'  
<img src="https://github.com/Born-Studios/knowledge-Base/assets/47042497/34cf0e90-f9a0-4dd2-844f-b8ffb5c5a990" width="50%">  

6. Click 'Advanced', then on the new Window 'Find Now'  
7. Look for your Log in name, click it and then hit 'OK'  
<img src="https://github.com/Born-Studios/knowledge-Base/assets/47042497/79b18290-a503-4022-97fb-567d18435b1d" width="50%"> 

8. That window should close, press 'OK' again on the previous window (Titled Select User or Group)  
9. Give yourself Full Control and Press 'OK'  
<img src="https://github.com/Born-Studios/knowledge-Base/assets/47042497/9685161f-1590-4e95-aa99-d6f7b7a15e41" width="50%">  

10. On the next screen (Advanced Security Settings for id_ed25519) Remove 'Users' and Authenticated Users. It should look like this at the end  
<img src="https://github.com/Born-Studios/knowledge-Base/assets/47042497/a122e831-6e02-4379-9025-de82a6149a8a" width="50%">  

11. Hit OK on the remaining windows and you should be done
  </details>
