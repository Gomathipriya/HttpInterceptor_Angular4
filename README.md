# HttpInterceptor_Angular4

To handle the absolute url error caused in angular universal by platform-server 

Create an Interceptor class 

<pre>

import { Injectable, Inject, Optional } from '@angular/core';
import { HttpInterceptor, HttpHandler, HttpRequest } from '@angular/common/http';

@Injectable()
export class UniversalInterceptor implements HttpInterceptor {

  constructor(@Optional() @Inject('serverUrl') protected serverUrl: string) {}

  intercept(req: HttpRequest any, next: HttpHandler) {
    const serverReq = !this.serverUrl ? req : req.clone({
      url: `${this.serverUrl}${req.url}`
    });
    console.log(serverReq.url);
    return next.handle(serverReq);

  }
  
 </pre>
 
 Import the interceptor in app.server.module.ts
 
 <pre>
 
 providers: [{
    provide: HTTP_INTERCEPTORS,
    useClass: UniversalInterceptor,
    multi: true
  }],
  
  </pre>
  
  In server.ts include the below code
  
  <pre>
  
  res.render(join(DIST_FOLDER, 'browser','index.html'), {
        req: req,
        res: res,
        providers: [{
          provide: 'serverUrl',
          useValue: `${req.protocol}://${req.get('host')}`
        }]
      });
      
   </pre>
