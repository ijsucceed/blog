---
title: "How to Check empty input in Javascript"
description: How you can prevent user from submiting empty input 
date: 2018-05-19T15:38:49+01:00
draft: false
---

If you search on the internet on how to prevent a user from submitting empty data on a form, what you will mostly find is this.
```
var input = document.getElementById('user_input');

if ( input.value == "" ) 
{
  alert("Empty");
}
```
Now, while this work in most case, it's only half the solution. The problem with this is that it sees white space as a data. Remember in computer science space is consider a character, though invisible. So if you enter just a single or more white space on the input, it will pass the validation.

Some other suggestions I found decided to use the following.
```
var input = document.getElementById('user_input');

if ( input.value == "" || input.value.length == 0 ) 
{
  alert("Empty");
}
```
But this still does not solve the problem. Because .length count space character.
```
> " ".length
> 1
```
The way I was able to solve this problem was by using the *trim* function.
```
if ( input.value.trim() == "" ) 
{

}
```
So paradventure the user enter some white space, the trim function will remove every white space, which makes the input empty.

You can try a working sample below.
```
<script>
function validate() {
    var input = document.getElementById('user_input');

    if ( input.value.trim() == "" ) 
    {
        alert("Empty");
        return false;
    }
}
</script>

<form onsubmit="return validate()" action="" method="post">
    <input type="text" id='user_input' placeholder="username">
    <button type="submit" name="login_submit" class="btn"> submit </button>
</form>
```
Hope you got it.