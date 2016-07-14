---
title: Rules
description: This tutorial will show you how to use Auth0 rules to extend what Auth0 has to offer.
---

<%= include('../../_includes/_github', {
  link: 'https://github.com/auth0-samples/auth0-angularjs2-systemjs-sample/tree/master/06-Rules',
}) %>_


<%= include('../_includes/_rules-introduction') %>

### Create a rule

<%= include('../_includes/_rules-create-section') %>


### Test rule result

<%= include('../_includes/_rules-test-result-intro', { profilelink: '/quickstart/spa/angular2/04-user-profile' }) %>

```html
/* ===== ./profile_show.template.html ===== */
<div *ngIf="auth.authenticated() && auth.userProfile">
  <div class="row">
    <div class="col-md-6">
      <h3>Profile</h3>
      ...
      <p><strong>Country (added by rule): </strong> {{auth.userProfile.country}}</p>
    </div>
  </div>
</div>
<h4 *ngIf="!auth.authenticated()">You are not logged in, please click 'Log in' button to login</h4>
```

![Country rule sample](/media/articles/angularjs2/rule-country-show.png)

### Done!
That's it. You've just experienced how to implement a basic rule. This is just one of all the cool things you can do with them. Go ahead and create any that fits your needs.