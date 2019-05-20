#### #6
```html
<!-- Comment @dmkorol:
I noticed that "something-row" is applied for all `some-app-something` components.
Maybe, it would be better to move thouse css rules into component?
-->
<some-app-something class="something-row"></some-app-something>
```
---------------
Possible solution:
In most cases you can apply thouse rules to `:host` directly in compoinent.scss
```scss
:host { ... }
```
BUT if you have to add PREDEFINED class(es) for all components in the whole 
application use this approach 'cause it is a secure way and there are 
no side effects (nobody can accidentally delete your styles)
```html
<some-app-something></some-app-something>
```ts
@Component({
  selector:"class="something-row"
})
constructor(public breadcrumbService: BreadcrumbsService,
              private elementRef: ElementRef) {
	this.elementRef.nativeElement.classList.add('breadcrumbs-row');
}
```
***BEAWERE***: 
There is some tricky parts with usage `:host` and `@HostBinding`	
they can override class='' [ngClass]="..."
See details: https://github.com/angular/angular/issues/7289


#### #10

Possible solution for SomesDictionary is use `ENUM` instead of string in `CONST` declaration and
in `switch case` construction. It allows to make changes in one place if text will change.

from:
```ts
export const SomesDictionary: MetricDictionary[] = [
    {name: 'Two wrongs don't make a right.', value: 0},
];
```
to: 
```ts
export enum SomesDictionaryEnum {
    LettableArea = 'Lettable area'
}

export const SomesDictionary: MetricDictionary[] = [
    {name: SomesDictionaryEnum.LettableArea, value: 0},
];

case SomesDictionaryEnum.LettableArea:
  specialMetricsUnit.value = this.valueFormattingPipe.transform(
      specialMetricsObj.blablabla, 'TransformArea'
  );
  break;
```

#### #15

Possible refactoring:

1. Use https://prettier.io/docs/en/webstorm.html for appropriate code style.
2. Add correct typing for `value` instead of `any`.
3. Add check for statment `this.topList = value.lists;` 
  if `value` will be undefined we will received error to console.
4. Unsubscribe from `document.addEventListener`.
5. Add typing and description for `(e: any) => {}` function (what is e?).
6. Add description why we are using setTimeout and add undefined check for `e.detail`:
```ts
  setTimeout(() => {
    this.selectedRowLid = e.detail;
    this.isChangedRows = this.selectedRowLid.length > 0;
  }, 0);
```        
