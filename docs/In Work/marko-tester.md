---
layout: default
title: marko-tester
nav_order: 3
parent: In Work
---

# marko-tester

每增添一个新的component，都要写相应的test。

- 创建一个`test`文件夹在这个component里面，然后里面创建一个`index.spec.js`。

  这里面 `const { render, fixtures } = require('marko-tester')('../index.marko’);` 的解释：

  `marko-tester` returns a function for you to use. Pass a relative path to a marko component file and you will receive a `render` method and `fixtures` method/object. By default this will run JEST SnapShots test for the fixtures of the component.

  - `render` - a method that renders a component with a given input and mounts it to `document.body`. The mounted component instance is returned.
  - `fixtures` - an object that contains all fixtures that are found within the `__snapshots__`folder for the component. You can get content of a fixture by specifying a filename: `fixtures[FixtureFileName]`.

- 并列于`index.spec.js`，创建`default.json`和其他json文件，这是存放输入的mock数据的地方。里面是调用这个component的外层marko给这个component传入的所有参数。每个json文件里都各有这样一套参数。可以从browser里复制过来改一下。数据值具体是什么不重要，只要test能对得上就行。

- 如果这个component有`component.js`里的方法，那么需要在`index.spec.js`里写unit test：

  - 每一个判断都是一个`discribe('discription of the scenario' () => {methods});`

    这里面先定义`const component = render(fixtures.default);`，（只能是default？）这个component就是render出来的这个component，也就是`component.js`里的`this`。传给`this`的`input`里的内容都要在`default.json`中定义了才能用（`this.xxx`）。

  - 接下来定义我们所需的mock参数，可以定义成JSON object。log出来被测试方法里的传入参数，copy下来我们现在所测试的scenario下所需要的那部分JSON之后直接放到mock data里面。最外层的string用单引号，最里层的用双引号。

  - 然后调用`component.testFunc(mockData);`，之后判断语句如下：

    ```javascript
    it('this.state.msg should be "scheduledMsg"', () => {
      expect(component.state.msg).toEqual('scheduledMsg');
    });
    ```

    这里`toEqual()`也有`toBe()`之类的替换，具体待查。

  - 全判断完，describe结束之前，要有

    ```javascript
    afterEach(() => {
      component.destroy();
    });
    ```

    销毁这个component，不然会陷入死循环。

- 每次更改测试文件之后，运行`$ yarn test -u`注意加上update的选项。第一次运行test会自动生成`__snapshots__`文件夹，并把json文件移进去。

  

  