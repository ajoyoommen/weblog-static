---
layout: post
title: Actions inside an EmberJS component
date: 2015-12-17 14:14:23 +0530
category: EmberJS
tags:
    - EmberJS
    - JavaScript
author: Ajoy Oommen
published: true
---
I have been working on an application with a component that had an action which was not [bubbling](https://guides.emberjs.com/v1.10.0/templates/actions/#toc_action-bubbling). After some digging, I found [this](https://guides.emberjs.com/v1.10.0/components/handling-user-interaction-with-actions/):

> ... actions sent from inside a component are sent directly to the component's Ember.Component instance, and do not bubble.

So, this component by itself will neither handle the action nor bubble it through the routers.

    <script type="text/x-handlebars" id="components/post-summary">
      <h3 {{action "toggleBody"}}>{{title}}</h3>
      {{#if isShowingBody}}
        <p>{{{body}}}</p>
      {{/if}}
    </script>

Adding the following component will solve the issue.

    App.PostSummaryComponent = Ember.Component.extend({
      actions: {
        toggleBody: function() {
          this.toggleProperty('isShowingBody');
        }
      }
    });