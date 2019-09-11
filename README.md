
### Use theme by git submodule

Download theme：
git submodule update --remote <submodule_name> 

Modify theme: 
```bash
	# Modify theme first
	cd themes/your_theme
	git pull origin master
	vim something
	git commit -m "Info"
	git push
	
	cd document/
	git add
	git commit -m "Mod theme"
	git push
```
