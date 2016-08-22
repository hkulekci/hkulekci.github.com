---
layout: post
title: "No Directive annotation found" Error Problem on AngularJS 2
---

For a while, I have been trying to learn AngularJS 2 and typescript. I really like 
Last day, Some error occured which is "No Directive annotation found" and I don't
understand why but I spent lots of time on it. The error is : 

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

Firtsly, I didn't understand why this error occured. I was only triying to create
a derivative for handling click event. And this is my class : 

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
Source : http://arjunu.com/2016/01/basic-angular-2-off-click-directive/

After that, I thought that I had to define something to somewhere. And searched 
on Google. I found lots of web site and lots of comments about this situation or 
I thought that these comments was about the situation. But, they were not 
relevant. Because all of them, was talking about version difference or importing
problems. Finally, I came accros [this post](http://stackoverflow.com/a/34524321/721600)
and also found the solution, too. The problem is ";" (semicolon) as usually. 
I remove my semicolon after @Directive definition and problem solved.
