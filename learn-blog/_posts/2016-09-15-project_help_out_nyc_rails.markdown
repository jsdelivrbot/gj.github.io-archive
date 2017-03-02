---
layout: post
title:  "Project: Help Out NYC (Rails)"
date:   2016-09-15 21:00:00 +0000
---

<h2>Project</h2>
<p>For my Rails assessment project, I created Help Out NYC, a simple web app to help NYC residents advertise, search for, and sign up for local volunteer opportunities. The app allows users to:
<ul><li>sign up</li>
<li>log in and out</li>
<li>create new organizations and events</li>
<li>edit and delete organizations and events <em>that they created</em></li>
<li>view any particular organization, issue, event, or user</li>
<li>sign up for and resign from events that have not yet met their volunteer goal</li>
<li>delete their own user account</li></ul></p>

<h3>Setup</h3>
<p>To run the app on a local web server, clone <a href="https://github.com/gj/help-out-rails" target="_blank">the GitHub repository</a> down to your local machine, <code>cd</code> into the repo's local directory, run <code>bundle install</code>, run <code>rails s</code>, and then navigate to <a href="http://localhost:3000/" target="_blank">localhost:3000</a> in your web browser (preferably Chrome).</p>

<h3>Usage</h3>
<p>Aside from the homepage, you will be unable to access any pages except <a href="http://localhost:3000/users/sign_up" target="_blank">/users/sign_up</a> and <a href="http://localhost:3000/sign_in" target="_blank">/users/sign_in</a> until you sign in.</p>

<h4>Sign up</h4>

<h5>Manual</h5>
<p>Signing up manually will require the creation of a new user account with a unique email address and a password of eight or more characters.</p>

<h5>Via Facebook</h5>
<p>Alternatively, you may log in via Facebook (the path is <a href="http://localhost:3000/users/auth/facebook" target="_blank">/users/auth/facebook</a>), which will redirect you to a Facebook login screen (if you are not already logged in to Facebook) and then to an authorization screen on which you will grant permissions to the app. If you decline to provide your email address, which the app requires to process new user accounts, you will be redirected back to Facebook and re-prompted for that information. If you again choose to withhold your email address, the app will prompt you to enter an email account (but no password) and will not let you complete the new account process without one. Should you provide an email address via this tertiary process, it will be automatically associated with your Facebook account, and all future attempts to login via Facebook (sans any password) will succeed.</p>

<h4>User account types</h4>

<h5>Basic User</h5>
<p>Once logged in, basic users can create new organizations and opportunities, edit or delete organizations or opportunities *that they created*, register for opportunities that have not yet met their volunteer goal, cancel registrations if they can no longer attend, and delete their own account. Basic users can also view the show page for any opportunity, organization, or issue as well as indices of each object. Users may only view their own profile page.</p>

<h5>Admin User</h5>
<p>Once logged in, admin users have the same privileges as basic users with the following enhancements:
<ul><li>can create, edit, or delete *any* organization, opportunity, or issue</li>
<li>can delete any basic user's account, but cannot delete the account of another admin</li>
<li>can view the index of all users as well as any user's profile page</li></ul></p>

<h3>Dummy (and real!) data</h3>
<p>For testing purposes, the project comes with <a href="https://github.com/gj/help-out-rails/blob/master/db/seeds.rb" target="_blank">a seed file</a> that contains code to populate the app's database (<a href="https://github.com/gj/help-out-rails/blob/master/db/development.sqlite" target="_blank">db/development.sqlite</a>) with the following data:
<ul><li>965 real NYC organizations scraped from <a href="https://data.cityofnewyork.us/Social-Services/NYC-Women-s-Resource-Network-Database/pqg4-dm6b" target="_blank">the NYC Women's Resource Network Database</a></li>
<li>22 real issues scraped from the same database</li>
<li>5000 fake opportunities generated randomly</li>
<li>200 fake users generated from <a href="https://www.ssa.gov/OACT/babynames/decades/century.html" target="_blank">the U.S. Social Security Administration's list of "the 100 most popular given names for male and female babies born during the last 100 years, 1916-2015"</a></li></ul></p>
