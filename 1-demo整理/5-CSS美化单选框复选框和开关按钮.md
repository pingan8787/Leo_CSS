> 这里整理几个demo，有分PC端和移动端。  

# PC端

![美化 radio](http://p3nqtyvgo.bkt.clouddn.com/1.png)
```html
<!--有简单的背景动画：-->
<label class="bui-radios-label bui-radios-anim">
    <input type="radio" name="sex"/><i class="bui-radios"></i> 男
</label>
<label class="bui-radios-label bui-radios-anim">
    <input type="radio" checked="" name="sex"/><i class="bui-radios"></i> 女
</label>
<!--无动画：-->
<label class="bui-radios-label">
    <input type="radio" name="sex"/><i class="bui-radios"></i> 男
</label>
<label class="bui-radios-label">
    <input type="radio" name="sex"/><i class="bui-radios"></i> 女
</label>
<!--禁用状态（disabled）：-->
<label class="bui-radios-label">
    <input type="radio" disabled="" name="sex"/><i class="bui-radios"></i> 不男不女
</label>
<label class="bui-radios-label">
    <input type="radio" checked="" disabled="" name="ddd"/><i class="bui-radios"></i> 不男不女
</label>
```

![美化 checkbox](http://p3nqtyvgo.bkt.clouddn.com/2.png)
```html
<!--有简单的背景动画：-->
<label class="bui-checkbox-label  bui-checkbox-anim">
    <input type="checkbox" checked="" name="sex"/><i class="bui-checkbox"></i> 男
</label>
<label class="bui-checkbox-label  bui-checkbox-anim">
    <input type="checkbox" name="sex"/><i class="bui-checkbox"></i> 女
</label>
<!--无动画：-->
<label class="bui-checkbox-label">
    <input type="checkbox" checked="" name="sex"/><i class="bui-checkbox"></i> 男
</label>
<label class="bui-checkbox-label">
    <input type="checkbox" name="sex"/><i class="bui-checkbox"></i> 女
</label>
<!--禁用状态（disabled）：-->
<label class="bui-checkbox-label">
    <input type="checkbox" disabled="" name="sex"/><i class="bui-checkbox"></i> 不男不女
</label>
<label class="bui-checkbox-label">
    <input type="checkbox" checked="" disabled="" name="ddd"/><i class="bui-checkbox"></i> 不男不女
</label>
```

![用 checkbox 模拟 switch](http://p3nqtyvgo.bkt.clouddn.com/3.png)
```html
<!--简单的过渡动画：-->
<label class="bui-switch-label bui-switch-anim">
    <input type="checkbox" name="s"/><i class="bui-switch"></i>
</label>
<label class="bui-switch-label bui-switch-anim">
    <input type="checkbox" checked="" name="s"/><i class="bui-switch"></i>
</label>
<!--简单的背景动画：-->
<label class="bui-switch-label bui-switch-animbg">
    <input type="checkbox" name="s"/><i class="bui-switch"></i>
</label>
<label class="bui-switch-label bui-switch-animbg">
    <input type="checkbox" checked="" name="s"/><i class="bui-switch"></i>
</label>
<!--无动画：-->
<label class="bui-switch-label ">
    <input type="checkbox" name="s"/><i class="bui-switch"></i>
</label>
<label class="bui-switch-label ">
    <input type="checkbox" checked="" name="s"/><i class="bui-switch"></i>
</label>
<!--禁用状态（disabled）：-->
<label class="bui-switch-label">
    <input type="checkbox" disabled="" name="sex"/><i class="bui-switch"></i> 不男不女
</label>
<label class="bui-switch-label">
    <input type="checkbox" checked="" disabled="" name="ddd"/><i class="bui-switch"></i> 不男不女
</label>
```

## SCSS代码
```css
/* radio */
label.bui-radios-label {
  input {
    position: absolute;
    opacity: 0;
    visibility: hidden;
  }
  .bui-radios {
    display: inline-block;
    position: relative;
    width: 13px;
    height: 13px;
    background: #FFFFFF;
    border: 1px solid #979797;
    border-radius: 50%;
    vertical-align: -2px;
  }
  input:checked + .bui-radios:after {
    position: absolute;
    content: "";
    width: 7px;
    height: 7px;
    background-color: #fff;
    border-radius: 50%;
    top: 3px;
    left: 3px;
  }
  input:checked + .bui-radios {
    background: #00B066;
    border: 1px solid #00B066;

  }
  input:disabled + .bui-radios {
    background-color: #e8e8e8;
    border: solid 1px #979797;
  }
  input:disabled:checked + .bui-radios:after {
    background-color: #c1c1c1;
  }
  &.bui-radios-anim .bui-radios {
    -webkit-transition: background-color ease-out .3s;
    transition: background-color ease-out .3s;

  }
}


/* checkbox */
label.bui-checkbox-label {
  input {
    position: absolute;
    visibility: hidden;
    opacity: 0;
  }
  .bui-checkbox {
    display: inline-block;
    position: relative;
    width: 13px;
    height: 13px;
    background: #FFFFFF;
    border: 1px solid #979797;
    border-radius: 2px;
    vertical-align: -2px;
  }
  input:checked + .bui-checkbox:after {
    color: #FFFFFF;
    font-family: iconfont;
    content: "\e600";
    width: 13px;
    height: 13px;
    font-size: 13px;
    line-height: 13px;
    position: absolute;
    top: 1px;
    left: 0;
  }
  input:checked + .bui-checkbox {
    background: #00B066;
    border: 1px solid #00B066;

  }
  input:disabled + .bui-checkbox {
    background-color: #e8e8e8;
    border: solid 1px #979797;
  }
  input:disabled:checked + .bui-checkbox:after {
    color: #c1c1c1;
  }
  &.bui-checkbox-anim .bui-checkbox {
    -webkit-transition: background-color ease-out .3s;
    transition: background-color ease-out .3s;

  }
}

@mixin borderRadius($radius:20px) {

  border-radius: $radius;
  border-top-left-radius: $radius;
  border-top-right-radius: $radius;
  border-bottom-left-radius: $radius;
  border-bottom-right-radius: $radius;
}

$duration: .4s;
$checkedColor: #64bd63;
label.bui-switch-label {
  input {
    position: absolute;
    opacity: 0;
    visibility: hidden;
  }
  input:checked {
    border-color: $checkedColor;
    box-shadow: $checkedColor 0 0 0 16px inset;
    background-color: $checkedColor;
    &:before {
      left: 27px;
    }
  }
  input:disabled + .bui-switch {
    background-color: #e8e8e8;
    border: solid 1px #dfdfdf;
    &:before {
      background-color: #c1c1c1;
    }
  }
  input:disabled:checked + .bui-switch {
      background-color: #e8e8e8;
    box-shadow: #e8e8e8 0 0 0 16px inset;
      border: solid 1px #dfdfdf;
      &:before {
        background-color: #c1c1c1;
      }
    }
  .bui-switch {
    width: 50px;
    height: 25px;
    position: relative;
    border: 1px solid #dfdfdf;
    background-color: #fdfdfd;
    box-shadow: #dfdfdf 0 0 0 0 inset;
    @include borderRadius();
    background-clip: content-box;
    display: inline-block;
    -webkit-appearance: none;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
    outline: none;
    &:before {
      content: '';
      width: 23px;
      height: 23px;
      position: absolute;
      top: 1px;
      left: 1px;
      @include borderRadius();
      background-color: #fff;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.4);
    }

  }
  input:checked + .bui-switch {
    border-color: $checkedColor;
    box-shadow: $checkedColor 0 0 0 16px inset;
    background-color: $checkedColor;
    &:before {
      left: 27px;
    }

  }
  &.bui-switch-anim {
    -webkit-transition: border cubic-bezier(0, 0, 0, 1) $duration, box-shadow cubic-bezier(0, 0, 0, 1) $duration;
    transition: border cubic-bezier(0, 0, 0, 1) $duration, box-shadow cubic-bezier(0, 0, 0, 1) $duration;
    .bui-switch:before {
      -webkit-transition: left 0.3s;
      transition: left 0.3s;
    }
    input:checked + .bui-switch {
      box-shadow: $checkedColor 0 0 0 16px inset;
      background-color: $checkedColor;
      -webkit-transition: border ease $duration, box-shadow ease $duration, background-color ease $duration*3;
      transition: border ease $duration, box-shadow ease $duration, background-color ease $duration*3;
      &:before {
        -webkit-transition: left 0.3s;
        transition: left 0.3s;
      }
    }
  }
  &.bui-switch-animbg {
    -webkit-transition: background-color ease $duration;
    transition: background-color ease $duration;
    .bui-switch:before {
      -webkit-transition: left 0.3s;
      transition: left 0.3s;
    }
    input:checked + .bui-switch {
      box-shadow: #dfdfdf 0 0 0 0 inset;
      background-color: $checkedColor;
      -webkit-transition: border-color $duration, background-color ease $duration;
      transition: border-color $duration, background-color ease $duration;
      &:before {
        -webkit-transition: left 0.3s;
        transition: left 0.3s;
      }
    }

  }
}
```