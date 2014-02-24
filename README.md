# Passport-CAS

[Passport](http://passportjs.org/) strategy for authenticating with [CAS](http://www.jasig.org/cas)
using the OAuth 2.0 API.

This module lets you authenticate using CAS in your Node.js applications.
By plugging into Passport, CAS authentication can be easily and
unobtrusively integrated into any application or framework that supports
[Connect](http://www.senchalabs.org/connect/)-style middleware, including
[Express](http://expressjs.com/).

## Install

    $ npm install passport-cas

## Usage

#### Configure Strategy

The CAS authentication strategy authenticates users using a CAS account
and OAuth 2.0 tokens.  The strategy requires a `verify` callback, which accepts
these credentials and calls `done` providing a user, as well as `options`
specifying a client ID, client secret, and callback URL.

    passport.use(new CasStrategy({
        clientID: CAS_CLIENT_ID,
        clientSecret: CAS_CLIENT_SECRET,
        callbackURL: "http://127.0.0.1:3000/auth/cas/callback"
      },
      function(accessToken, refreshToken, profile, done) {
        User.findOrCreate({ casId: profile.id }, function (err, user) {
          return done(err, user);
        });
      }
    ));

#### Authenticate Requests

Use `passport.authenticate()`, specifying the `'cas'` strategy, to
authenticate requests.

For example, as route middleware in an [Express](http://expressjs.com/)
application:

    app.get('/auth/cas',
      passport.authenticate('cas'));

    app.get('/auth/cas/callback',
      passport.authenticate('cas', { failureRedirect: '/login' }),
      function(req, res) {
        // Successful authentication, redirect home.
        res.redirect('/');
      });
