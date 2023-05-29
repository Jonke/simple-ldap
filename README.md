# Simple LDAP client library for Ldap3

A ldap client library that wraps [ldap3](https://github.com/inejge/ldap3) to make it easy to use.

### Status of the project
Currently this is in early alpha stage. Library only support use asynchronously. 

## Usage
```
cargo add simple-ldap
```


## Examples

```rust
use simple-ldap::{LdapClient,Error,EqFilter};
use simple-ldap::ldap3::Scope;

#[tokio::main]
fn main() -> Result<()> {
    let mut client = LdapClient::from("ldap://localhost:2389", "cn=admin", "password")?;
    
    let object_filter = EqFilter {
        attribute: "objectClass".to_string(),
        value: "person".to_string()
    }

    let mail_filter = EqFilter {
        attribute: "mail".to_string(),
        value: "some@mail.com".to_string(),
    };

    let filter = AndFilter {
        filters: vec![
            Box::new(object_filter),
            Box::new(mail_filter),
        ],
    };

    let result: Result<User,Error> =  client.search("dc=example,dc=com",Scope::Subtree,&filter, vec!["cn","sn","c","l"]).await;
    Ok(ldap.unbind()?)
}
```