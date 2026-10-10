---
title: "A better Catalyst Controller block"
date: 2012-06-14
tags: ["perl", "yapc"]
original_url: https://deafferret.tumblr.com/post/25108497658/a-better-catalyst-controller-block
templateEngineOverride: md
---

*Originally posted to [deafferret](https://deafferret.tumblr.com/post/25108497658/a-better-catalyst-controller-block) on 2012-06-14.*

So, stick this in your pipe and smoke it:

    sub some_catalyst_action :Local {
       my( $self , $c ) = @_;
       try {
          my $validated_params = validate_request_params( $c->request->params );
          my $step_one_result = $c->model(BusinessLogic)->do_step_one( $session , $validated_params );
          my $step_two_result = $c->model(BusinessLogic)->do_step_two( $step_one_result, $session );
       }
       catch { 
          $c->detach( error , [ $_ ] ) };
       }
    }
