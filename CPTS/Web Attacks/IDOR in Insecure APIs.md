## Identifying Insecure APIs
![](Pasted%20image%2020260723124059.png)
![](Pasted%20image%2020260723124111.png)
![](Pasted%20image%2020260723124351.png)

```json
{ 
	"uid": 1, 
	"uuid": "40f5888b67c748df7efba008e7c2f9d2", 
	"role": "employee", 
	"full_name": "Amy Lindon", 
	"email": "a_lindon@employees.htb", 
	"about": "A Release is like a boat. 80% of the holes plugged is not good enough." 
}
```
## Exploiting Insecure APIs

There are a few things we could try in this case:

1. Change our `uid` to another user's `uid`, such that we can take over their accounts
2. Change another user's details, which may allow us to perform several web attacks
3. Create new users with arbitrary details, or delete existing users
4. Change our role to a more privileged role (e.g. `admin`) to be able to perform more actions
![](Pasted%20image%2020260723124625.png)
![](Pasted%20image%2020260723124646.png)
![](Pasted%20image%2020260723124712.png)
![](Pasted%20image%2020260723124729.png)
