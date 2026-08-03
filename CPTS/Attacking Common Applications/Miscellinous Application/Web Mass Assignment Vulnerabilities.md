## Exploiting Mass Assignment Vulnerability

![](Web%20Mass%20Assignment%20Vulnerabilities-20260802-215151.png)

Reviewing the python code of the `/opt/asset-manager/app.py` file reveals the following snippet.

```python
for i,j,k in cur.execute('select * from users where username=? and password=?',(username,password)): 
	if k: 
		session['user']=i 
		return redirect("/home",code=302) 
	else: 
		return render_template('login.html',value='Account is pending for approval')
```

```python
try: 
	if request.form['confirmed']: 
		cond=True 
except: 
			cond=False 
with sqlite3.connect("database.db") as con: 
	cur = con.cursor() 
	cur.execute('select * from users where username=?',(username,)) 
	if cur.fetchone(): 
		return render_template('index.html',value='User exists!!') 
	else: 
		cur.execute('insert into users values(?,?,?)',(username,password,cond)) 
		con.commit() 
		return render_template('index.html',value='Success!!')
```

Using Burp Suite, we can capture the HTTP POST request to the `/register` page and set the parameters `username=new&password=test&confirmed=test`.
![](Web%20Mass%20Assignment%20Vulnerabilities-20260802-215932.png)

We can now try to log in to the application using the `new:test` credentials.
![](Web%20Mass%20Assignment%20Vulnerabilities-20260802-215954.png)