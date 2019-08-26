
## hello blog

### themes采用git submodule的方式引入，保证通信
因为 git clone方式 会导致引入的theme无法上传至 github

下载主题：
git submodule update --remote <submodule_name> 

修改themes : 
```bash
	# 先push theme 突然再push Hexo
	cd themes/your_theme
	git pull origin master
	vim something
	git commit -m "Info"
	git push
	
	cd Hexo/
	# 此时 themes 被修改
	git add
	git commit -m "Mod theme"
	git push
```