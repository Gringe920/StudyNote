css实现360度旋转

```html
<div class="roll"></div>
```


```css
.roll{
	-webkit-animation: roll 1s linear infinite;
    -moz-animation: roll 1s linear infinite;
    -o-animation: roll 1s linear infinite;
}

@-webkit-keyframes roll {
    0% {
        -webkit-transform: rotate(0deg);
    }
    50% {
        -webkit-transform: rotate(180deg);
    }
    100% {
        -webkit-transform: rotate(360deg);
    }
}

@-moz-keyframes roll {
    0% {
        -webkit-transform: rotate(0deg);
    }
    50% {
        -webkit-transform: rotate(180deg);
    }
    100% {
        -webkit-transform: rotate(360deg);
    }
}

@-o-keyframes roll {
    0% {
        -webkit-transform: rotate(0deg);
    }
    50% {
        -webkit-transform: rotate(180deg);
    }
    100% {
        -webkit-transform: rotate(360deg);
    }
}

@keyframes roll {
    0% {
        -webkit-transform: rotate(0deg);
    }
    50% {
        -webkit-transform: rotate(180deg);
    }
    100% {
        -webkit-transform: rotate(360deg);
    }
}
```