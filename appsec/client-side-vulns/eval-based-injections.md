# Eval-based Injections

If you can control a string that is dynamically evaluated, you have hit the jackpot and may proceed to inject arbitrary code of your choosing. This should be a rare occurrence.

```jsx
function antiPattern() {
  eval(this.state.attacker_supplied);
}
// Or even crazier
fn = new Function("..." + attacker_supplied + "...");
fn();

// Example
const addition = new Function(‘a’, ‘b’, ‘return a+b’);
addition(1, 1)

// Так же нельзя допускать, чтобы строки попадали 
// в setInterval() или setTimeout() как аргумент
setTimeout(“console.log(1+1)”, 1000);

```

Соответственно, ищем использование:

```jsx
eval()
new Function()
setTimeout()
setInterval()
```
