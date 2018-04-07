1. Install
	$ sudo apt-get install git

2. Configuration
	$ git config --global user.name "hytxx"
	$ git config --global user.email "hyt.lyy@gmail.com"

3. Get the information 
	$ git config --list

4. Create Respository
	$ git init

5. 关联远程库
	$ git remote add origin git@github.com:hytxx/Configuration.git

6. 合并远程库和本地库
	$ git pull --rebase origin master

7. 添加提交
	$ git add readme.txt
	$ git commit -m "write a readme file"

8. 查看
	$ git status
	$ git log
	$ git log --pretty=oneline
	$ git diff readme.txt

9. 版本退回
	$ git git reset --hard HEAD^

10. 推送远程库
	$ git push -u origin master

11. 删除
	$ git rm readme.txt
	$ git commit -m "remove readme.txt"
