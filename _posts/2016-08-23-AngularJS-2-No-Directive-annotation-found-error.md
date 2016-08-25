---
layout: post
title: Angular 2 Directive Error
---

For a while, I have been trying to learn AngularJS 2 and typescript. I really 
like them. Last day, an error occured which is "No Directive annotation found" 
while I was trying to create a directive and I don't understand exception why 
throwing but I spent lots of time on it to solve. The error is : 

```
members:16 Error: (SystemJS) No Directive annotation found on offClickDirective
  Error: No Directive annotation found on offClickDirective
      at new BaseException (http://localhost:3000/node_modules/@angular/compiler/bundles/compiler.umd.js:5116:27)
      at DirectiveResolver.resolve (http://localhost:3000/node_modules/@angular/compiler/bundles/compiler.umd.js:12790:23)
      at CompileMetadataResolver.getDirectiveMetadata (http://localhost:3000/node_modules/@angular/compiler/bundles/compiler.umd.js:13097:55)
      at addDirective (http://localhost:3000/node_modules/@angular/compiler/bundles/compiler.umd.js:13367:37)
      at eval (http://localhost:3000/node_modules/@angular/compiler/bundles/compiler.umd.js:13376:77)
      at Array.forEach (native)
      at CompileMetadataResolver._getTransitiveViewDirectivesAndPipes (http://localhost:3000/node_modules/@angular/compiler/bundles/compiler.umd.js:13376:41)
      at eval (http://localhost:3000/node_modules/@angular/compiler/bundles/compiler.umd.js:13348:78)
      at Array.forEach (native)
      at CompileMetadataResolver._verifyModule (http://localhost:3000/node_modules/@angular/compiler/bundles/compiler.umd.js:13348:43)
  Evaluating http://localhost:3000/dist/main.js
  Error loading http://localhost:3000/dist/main.js
```

I also imported necessary component and classes and this is my component file 
content: 

```
import { Directive, Input, Host } from '@angular/core';

@Directive({
  selector: '[offClick]',
  //inputs: ['offClick'],
  host: {
    '(click)':'onClick($event)'
  }
});

export class offClickDirective {
  @Input('offClick') offClickHandler;
  
  constructor() {}

  ngOnInit() {
    let self=this;
    setTimeout(() => {document.addEventListener('click', self.offClickHandler);}, 0);
  }
  
  ngOnDestroy() {
    let self=this;
    document.removeEventListener('click', self.offClickHandler);    
  }

  closeAllDropdown($event) {
    if ($event.target.classList.contains('dropdown-toggle')) {
        return
      }
      const dropdowns = document.querySelectorAll('.dropdown')
      for (let i = 0; i < dropdowns.length; i++) {
        dropdowns[i].classList.remove('open')
      }
  }

  onClick($event) {
    this.closeAllDropdown($event);
  }
}
```

After that, I thought that I had to define something to somewhere for example 
systemjs.config.js, ...etc. And I searched it on Google. I found lots of issues
and lots of comments about this situation. But, in fact, they were not relevant.
Because all of them, was talking about version difference or importing problems.
I had already imported all the necessary libraries. Finally, I came accros 
[this post](http://stackoverflow.com/a/34524321/721600) and also found the 
solution, too. The problem is ";" (semicolon) as usually. I remove my semicolon 
after `@Directive` definition and problem solved. This is the last version of 
the `@Directive` part of my code.

```
@Directive({
  selector: '[offClick]',
  //inputs: ['offClick'],
  host: {
    '(click)':'onClick($event)'
  }
}) // removed semicolon was here
```
