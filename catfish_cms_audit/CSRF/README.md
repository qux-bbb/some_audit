## Catfish CMS 4.9.0 CSRF

CSRF vulnerability is caused by adding user without adding role permission filtering.  

Normally the system has only one super administrator whose type is 1 and other user. Like this,  
![user init](./images/user_init.png)  
We can see that admin11 is the super administrator(type=1). admin11 can add "Administrator"(type=3), "Editor"(type=5), "Author"(type=6)

After logging in as an administrator, view the current background user as follows,  
![page user](./images/page_user.png)  


We can add a super administrator with type 1 through CSRF vulnerability, as follows,  
1. Add user, select the role as Author  
![add author user](./images/add_author_user.png)  
2. Use burpsuite to intercept request, click "Add to", you can see that juese=6 and the type is "author"  
![user_type 6](./images/type_6.png)  
Make the juese=6 to juese=1 and forward the request  
![user_type 1](./images/type_1.png)  
No new user with the role of "author" is generated in the background users.  
![no new author user](./images/no_new_author.png)  

3. Look at the database and find an extra super administrator user(admin_test, type=1):  
![database new super admin](./images/database_new_super_admin.png)  

4. Log in with admin_test.  
![admin_test login](./images/admin_test_login.png)  
We can successfully login and manage CMS, and the original super administrator admin11 can't find the user admin_test without looking at the database.  