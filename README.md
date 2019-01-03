
## hello blog

### themes采用git submodule的方式引入，保证通信
git clone会导致引入的theme无法上传至 github

修改themes : 
```bash
	cd themes/your_theme
	vim ...
	git commit -m "Info"
	git push
	
	cd Hexo/
	# 此时 themes 被修改
	git add
	git commit -m "Mod theme"
	git push
```