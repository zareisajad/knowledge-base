TIL that you can have same urls and but accept different HTTP methods.
this is actually how REST APIs work and should be designed.


Example:

```
/addresses/<int:address_id>
```

can support:

| Method | Meaning        |
| ------ | -------------- |
| GET    | get address    |
| PATCH  | update address |
| DELETE | delete address |
