## Use cases
Based on the way output can be done in C, the developer tools support output formatting with
specifiers, like this:

<table>
    <tr>
        <td>## Format specifier</td>
        <td>## Description</td>
    </tr>

    <tr>
        <td>**`%s`**</td>
        <td>String</td>
    </tr>
    <tr>
        <td>**`%d`** or **`%i`**</td>
        <td>Integer</td>
    </tr>
    <tr>
        <td>**`%f`**</td>
        <td>Floating point value</td>
    </tr>
    <tr>
        <td>**`%o`**</td>
        <td>Expandable raw DOM element</td>
    </tr>
    <tr>
        <td>**`%O`**</td>
        <td>Expandable Javascript object</td>
    </tr>
    <tr>
        <td>**`%c`**</td>
        <td>Will format the output according to the provided CSS style</td>
    </tr>
</table>

#

## **`%c`** example

```javascript
console.log(‘%cThis is styled content’, ‘<style_parameters>’);
```

The **`​Shift + Enter`** combo will allow the insertion of a multiline code snippet into the console.
For executing the inserted snippet, hit `​Enter`.
​

## Commands
Opening it

Mac: **`Alt` + `Cmd` + `i`**

Windows & Linux: **`Ctrl` + `Shift` + `i`**

#

```javascript
console.log(object);
```
Will output the object to the console, plain and simple.

#

```javascript
console.assert(<condition>, ‘Display if the condition is ​​false’);
```
Will check the validity of the condition, and display the result if the condition is false.

#

```javascript
console.clear();
```
Will clear the output in the console.

#

```javascript
console.count(‘Text label for the count display’);
```
Will count the number of passes on the same line for the execution.

#

```javascript
console.dir(<object_to_output>);
```
Will output the formatted object parameter to the console, represented as a DOM object.

```javascript
console.dirxml(<object_to_output>);
```
Will output to the console the xml representation of the object.

#

```javascript
console.error(object);
```
Will output to the console the object, in the same way as ​console.log(object)​​ would, but with an added stack trace to the output.

#

```javascript
console.dirxml(<object_to_output>);
```
Will output to the console the xml representation of the object.

```javascript
console.group(group_label);
console.groupEnd();
```

This will group the following ​console statements which are used in between thee 2 expressions.

#

```javascript
console.groupCollapsed(group_label); ​​
```

can be used for displaying the group elements in a collapsed group.

#

```javascript
console.info(<object_to_output>);
```
Will output to the console the object, in the same way the ​console.log(object)​​ would, but with a info icon.

#

```javascript
console.profile(profile_label);
console.profileEnd();
```
This can serve as a benchmark tool for the statements in between them. It will start a CPU profiling tool and end its functioning.

#

```javascript
console.table(structure);
```
This will display the data structure as a table.

#

```javascript
console.time(timer_label);
console.timeEnd(timer_label);
```
This will time the execution of the code snippet in between these 2 statements.

#

```javascript
console.timeline(timeline_label);
console.timelineEnd(timeline_label);
```
This will make a timeline in the chrome developer tools for the code snippet in between them.

#

To manually add an event to a generated timeline, use the following statement:
```javascript
console.timeStamp(timestamp_label);
console.trace();
```

#

This will print a stack trace for the execution moment.
```javascript
console.warn(<object_to_output>);
```
Will output to the console the object, in the same way the ​console.log(object)​​ would, but with a warning icon.

## Also
Nicely illustrated in the [Chrome documentation​](https://developers.google.com/web/tools/chrome-devtools/console/api?utm_campaign=2016q3&utm_medium=redirect&utm_source=dcc).

Further reading ​[Chrome dev tools documentation​](https://developers.google.com/web/tools/chrome-devtools/?utm_source=dcc&utm_medium=redirect&utm_campaign=2016q3).
