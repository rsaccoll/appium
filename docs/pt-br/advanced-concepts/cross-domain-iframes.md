## Automação de Cross-domain iFrame 

[Same-origin policy](https://en.wikipedia.org/wiki/Same-origin_policy) impede o Appium de automatizar iFrames que tenham domínios defirentes do principal.

### Subdomain "Gambi"
Se o parent e o IFrame estão sob o mesmo domínio (ex: `site.com` s `shop.site.com`), voce pode
ajustar `document.domain` em ambos parent para cada iFrame possuir o mesmo domínio. Isso resolve o same-origin policy issue e permite a automação. Por examplo:

Parent:
```
<html>
  <head>
    <script>
      document.domain = 'site.com';
    </script>
  </head>
  <body>
    <iframe src="http://shop.site.com" width="200" height="200"></iframe>
  </body>
</html>
```

Child iFrame:
```
<html>
  <head>
    <script>
      document.domain = 'site.com';
    </script>
  </head>
  <body>
    <p>This is an iFrame!</p>
  </body>
</html>
```
