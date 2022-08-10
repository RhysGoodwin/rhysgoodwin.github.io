---
id: 952
title: 'WordPress: wp_signon() &#8211; $current_user is not populated'
date: '2010-12-28T11:09:23+13:00'

excerpt: 'When using the WordPress wp_signon() function the global user variables such as $current_user and $user_ID don''t get populated until after the page has refreshed.  Here''s how to work around it.'
layout: single
guid: 'https://blog.rhysgoodwin.com/?p=952'
permalink: /programming/wordpress-wp_signon-current_user-is-not-populated/
wp-syntax-cache-content:
    - "a:1:{i:1;s:13266:\"\n<div class=\"wp_syntax\" style=\"position:relative;\"><table><tr><td class=\"code\"><pre class=\"php\" style=\"font-family:monospace;\"><span style=\"color: #000000; font-weight: bold;\">&lt;?php</span>\n_my_newuser<span style=\"color: #009900;\">&#40;</span><span style=\"color: #0000ff;\">'joe'</span><span style=\"color: #339933;\">,</span><span style=\"color: #0000ff;\">'itsasecret'</span><span style=\"color: #339933;\">,</span><span style=\"color: #0000ff;\">'joe@joe.com'</span><span style=\"color: #339933;\">,</span><span style=\"color: #0000ff;\">'Joe'</span><span style=\"color: #009900;\">&#41;</span><span style=\"color: #339933;\">;</span>\n&nbsp;\n_my_user_login<span style=\"color: #009900;\">&#40;</span><span style=\"color: #0000ff;\">'joe'</span><span style=\"color: #339933;\">,</span><span style=\"color: #0000ff;\">'itsasecret'</span><span style=\"color: #009900;\">&#41;</span><span style=\"color: #339933;\">;</span>\n&nbsp;\n_my_commit_post<span style=\"color: #009900;\">&#40;</span><span style=\"color: #000088;\">$newpost</span><span style=\"color: #009900;\">&#41;</span><span style=\"color: #339933;\">;</span>\n&nbsp;\n_my_current_user<span style=\"color: #009900;\">&#40;</span><span style=\"color: #009900;\">&#41;</span><span style=\"color: #339933;\">;</span>\n&nbsp;\n<span style=\"color: #666666; font-style: italic;\">//Register a new user</span>\n<span style=\"color: #000000; font-weight: bold;\">function</span> _my_newuser<span style=\"color: #009900;\">&#40;</span><span style=\"color: #000088;\">$username</span><span style=\"color: #339933;\">,</span><span style=\"color: #000088;\">$password</span><span style=\"color: #339933;\">,</span><span style=\"color: #000088;\">$email</span><span style=\"color: #339933;\">,</span><span style=\"color: #000088;\">$nickname</span><span style=\"color: #009900;\">&#41;</span>\n<span style=\"color: #009900;\">&#123;</span>\n\t<span style=\"color: #000000; font-weight: bold;\">global</span> <span style=\"color: #000088;\">$wpdb</span><span style=\"color: #339933;\">;</span>\n\t<span style=\"color: #000088;\">$user_login</span> <span style=\"color: #339933;\">=</span> <span style=\"color: #000088;\">$wpdb</span><span style=\"color: #339933;\">-&gt;</span><span style=\"color: #004000;\">escape</span><span style=\"color: #009900;\">&#40;</span> <span style=\"color: #000088;\">$username</span> <span style=\"color: #009900;\">&#41;</span><span style=\"color: #339933;\">;</span>\n\t<span style=\"color: #000088;\">$user_pass</span> <span style=\"color: #339933;\">=</span> <span style=\"color: #000088;\">$password</span><span style=\"color: #339933;\">;</span>\n\t<span style=\"color: #000088;\">$nickname</span> <span style=\"color: #339933;\">=</span> <span style=\"color: #000088;\">$nickname</span><span style=\"color: #339933;\">;</span>\n\t<span style=\"color: #000088;\">$user_email</span> <span style=\"color: #339933;\">=</span> <span style=\"color: #000088;\">$username</span><span style=\"color: #339933;\">;</span>\n\t<span style=\"color: #000088;\">$userdata</span> <span style=\"color: #339933;\">=</span> <span style=\"color: #990000;\">compact</span><span style=\"color: #009900;\">&#40;</span><span style=\"color: #0000ff;\">'user_login'</span><span style=\"color: #339933;\">,</span> <span style=\"color: #0000ff;\">'user_email'</span><span style=\"color: #339933;\">,</span> <span style=\"color: #0000ff;\">'user_pass'</span><span style=\"color: #339933;\">,</span> <span style=\"color: #0000ff;\">'first_name'</span><span style=\"color: #339933;\">,</span><span style=\"color: #0000ff;\">'nickname'</span> <span style=\"color: #009900;\">&#41;</span><span style=\"color: #339933;\">;</span>\n\t<span style=\"color: #b1b100;\">return</span> wp_insert_user<span style=\"color: #009900;\">&#40;</span><span style=\"color: #000088;\">$userdata</span><span style=\"color: #009900;\">&#41;</span><span style=\"color: #339933;\">;</span>\n<span style=\"color: #009900;\">&#125;</span>\n&nbsp;\n<span style=\"color: #666666; font-style: italic;\">//Log a user in and set them as current user</span>\n<span style=\"color: #000000; font-weight: bold;\">function</span> _my_user_login<span style=\"color: #009900;\">&#40;</span><span style=\"color: #000088;\">$username</span><span style=\"color: #339933;\">,</span><span style=\"color: #000088;\">$password</span><span style=\"color: #009900;\">&#41;</span>\n<span style=\"color: #009900;\">&#123;</span>\n\t<span style=\"color: #000088;\">$creds</span> <span style=\"color: #339933;\">=</span> <span style=\"color: #990000;\">array</span><span style=\"color: #009900;\">&#40;</span><span style=\"color: #009900;\">&#41;</span><span style=\"color: #339933;\">;</span>\n\t<span style=\"color: #000088;\">$creds</span><span style=\"color: #009900;\">&#91;</span><span style=\"color: #0000ff;\">'user_login'</span><span style=\"color: #009900;\">&#93;</span> <span style=\"color: #339933;\">=</span> <span style=\"color: #000088;\">$username</span><span style=\"color: #339933;\">;</span>\n\t<span style=\"color: #000088;\">$creds</span><span style=\"color: #009900;\">&#91;</span><span style=\"color: #0000ff;\">'user_password'</span><span style=\"color: #009900;\">&#93;</span> <span style=\"color: #339933;\">=</span> <span style=\"color: #000088;\">$password</span><span style=\"color: #339933;\">;</span>\n\t<span style=\"color: #000088;\">$creds</span><span style=\"color: #009900;\">&#91;</span><span style=\"color: #0000ff;\">'remember'</span><span style=\"color: #009900;\">&#93;</span> <span style=\"color: #339933;\">=</span> <span style=\"color: #009900; font-weight: bold;\">true</span><span style=\"color: #339933;\">;</span>\n\t<span style=\"color: #000088;\">$user</span> <span style=\"color: #339933;\">=</span> wp_signon<span style=\"color: #009900;\">&#40;</span> <span style=\"color: #000088;\">$creds</span><span style=\"color: #339933;\">,</span> <span style=\"color: #009900; font-weight: bold;\">false</span> <span style=\"color: #009900;\">&#41;</span><span style=\"color: #339933;\">;</span>\n\twp_set_current_user<span style=\"color: #009900;\">&#40;</span><span style=\"color: #000088;\">$user</span><span style=\"color: #339933;\">-&gt;</span><span style=\"color: #004000;\">ID</span><span style=\"color: #009900;\">&#41;</span><span style=\"color: #339933;\">;</span> <span style=\"color: #666666; font-style: italic;\">//Here is where we update the global user variables</span>\n\t<span style=\"color: #b1b100;\">return</span> <span style=\"color: #000088;\">$user</span><span style=\"color: #339933;\">;</span>\n<span style=\"color: #009900;\">&#125;</span>\n&nbsp;\n<span style=\"color: #666666; font-style: italic;\">//Commits a new post to the DB</span>\n<span style=\"color: #000000; font-weight: bold;\">function</span> _my_commit_post<span style=\"color: #009900;\">&#40;</span><span style=\"color: #000088;\">$postdata</span><span style=\"color: #009900;\">&#41;</span>\n<span style=\"color: #009900;\">&#123;</span>\n\t<span style=\"color: #000000; font-weight: bold;\">global</span> <span style=\"color: #000088;\">$user_ID</span><span style=\"color: #339933;\">;</span>\n\t<span style=\"color: #000088;\">$new_post</span> <span style=\"color: #339933;\">=</span> <span style=\"color: #990000;\">array</span><span style=\"color: #009900;\">&#40;</span>\n\t\t<span style=\"color: #0000ff;\">'post_title'</span> <span style=\"color: #339933;\">=&gt;</span> <span style=\"color: #000088;\">$postdata</span><span style=\"color: #339933;\">-&gt;</span><span style=\"color: #004000;\">title</span><span style=\"color: #339933;\">,</span>\n\t\t<span style=\"color: #0000ff;\">'post_content'</span> <span style=\"color: #339933;\">=&gt;</span> <span style=\"color: #000088;\">$postdata</span><span style=\"color: #339933;\">-&gt;</span><span style=\"color: #004000;\">description</span><span style=\"color: #339933;\">,</span>\n\t\t<span style=\"color: #0000ff;\">'post_status'</span> <span style=\"color: #339933;\">=&gt;</span> <span style=\"color: #0000ff;\">'publish'</span><span style=\"color: #339933;\">,</span>\n\t\t<span style=\"color: #0000ff;\">'post_date'</span> <span style=\"color: #339933;\">=&gt;</span> <span style=\"color: #990000;\">date</span><span style=\"color: #009900;\">&#40;</span><span style=\"color: #0000ff;\">'Y-m-d H:i:s'</span><span style=\"color: #009900;\">&#41;</span><span style=\"color: #339933;\">,</span>\n\t\t<span style=\"color: #0000ff;\">'post_author'</span> <span style=\"color: #339933;\">=&gt;</span> <span style=\"color: #000088;\">$user_ID</span><span style=\"color: #339933;\">,</span>\n\t\t<span style=\"color: #0000ff;\">'post_type'</span> <span style=\"color: #339933;\">=&gt;</span> <span style=\"color: #0000ff;\">'post'</span><span style=\"color: #339933;\">,</span>\n\t\t<span style=\"color: #0000ff;\">'post_category'</span> <span style=\"color: #339933;\">=&gt;</span> <span style=\"color: #990000;\">array</span><span style=\"color: #009900;\">&#40;</span><span style=\"color: #cc66cc;\">0</span><span style=\"color: #009900;\">&#41;</span><span style=\"color: #339933;\">,</span>\n\t\t<span style=\"color: #0000ff;\">'tags_input'</span> <span style=\"color: #339933;\">=&gt;</span> <span style=\"color: #000088;\">$postdata</span><span style=\"color: #339933;\">-&gt;</span><span style=\"color: #004000;\">tags</span>\n\t<span style=\"color: #009900;\">&#41;</span><span style=\"color: #339933;\">;</span>\n\t<span style=\"color: #000088;\">$post_id</span> <span style=\"color: #339933;\">=</span> wp_insert_post<span style=\"color: #009900;\">&#40;</span><span style=\"color: #000088;\">$new_post</span><span style=\"color: #339933;\">,</span><span style=\"color: #009900; font-weight: bold;\">true</span><span style=\"color: #009900;\">&#41;</span><span style=\"color: #339933;\">;</span>\n<span style=\"color: #009900;\">&#125;</span>\n&nbsp;\n<span style=\"color: #666666; font-style: italic;\">//Print info about the user who is now logged in</span>\n<span style=\"color: #000000; font-weight: bold;\">function</span> _my_current_user<span style=\"color: #009900;\">&#40;</span><span style=\"color: #009900;\">&#41;</span>\n<span style=\"color: #009900;\">&#123;</span>\n\t<span style=\"color: #000000; font-weight: bold;\">global</span> <span style=\"color: #000088;\">$current_user</span><span style=\"color: #339933;\">,</span><span style=\"color: #000088;\">$user_ID</span><span style=\"color: #339933;\">;</span>\n&nbsp;\n\t<span style=\"color: #b1b100;\">if</span><span style=\"color: #009900;\">&#40;</span>is_user_logged_in<span style=\"color: #009900;\">&#40;</span><span style=\"color: #009900;\">&#41;</span><span style=\"color: #009900;\">&#41;</span>\n\t<span style=\"color: #009900;\">&#123;</span>\n\t\t<span style=\"color: #b1b100;\">echo</span> <span style=\"color: #0000ff;\">'&lt;br /&gt;User Logged in ok&lt;br /&gt;'</span><span style=\"color: #339933;\">;</span>\n\t\t<span style=\"color: #b1b100;\">echo</span> <span style=\"color: #0000ff;\">'User ID is: '</span><span style=\"color: #339933;\">.</span><span style=\"color: #000088;\">$user_ID</span><span style=\"color: #339933;\">.</span><span style=\"color: #0000ff;\">'&lt;br /&gt;'</span><span style=\"color: #339933;\">;</span>\n\t\t<span style=\"color: #b1b100;\">echo</span> <span style=\"color: #0000ff;\">'User login is: '</span><span style=\"color: #339933;\">.</span><span style=\"color: #000088;\">$current_user</span><span style=\"color: #339933;\">-&gt;</span><span style=\"color: #004000;\">user_login</span><span style=\"color: #339933;\">.</span><span style=\"color: #0000ff;\">'&lt;br /&gt;'</span><span style=\"color: #339933;\">;</span>\n\t<span style=\"color: #009900;\">&#125;</span>\n\t<span style=\"color: #b1b100;\">else</span>\n\t\t<span style=\"color: #b1b100;\">echo</span> <span style=\"color: #0000ff;\">'No user is logged in&lt; br/&gt;'</span><span style=\"color: #339933;\">;</span>\n<span style=\"color: #009900;\">&#125;</span>\n&nbsp;\n<span style=\"color: #000000; font-weight: bold;\">?&gt;</span></pre></td></tr></table><p class=\"theCode\" style=\"display:none;\">&lt;?php\r\n_my_newuser('joe','itsasecret','joe@joe.com','Joe');\r\n\r\n_my_user_login('joe','itsasecret');\r\n\r\n_my_commit_post($newpost);\r\n\r\n_my_current_user();\r\n\r\n//Register a new user\r\nfunction _my_newuser($username,$password,$email,$nickname)\r\n{\r\n\tglobal $wpdb;\r\n\t$user_login = $wpdb-&gt;escape( $username );\r\n\t$user_pass = $password;\r\n\t$nickname = $nickname;\r\n\t$user_email = $username;\r\n\t$userdata = compact('user_login', 'user_email', 'user_pass', 'first_name','nickname' );\r\n\treturn wp_insert_user($userdata);\r\n}\r\n\r\n//Log a user in and set them as current user\r\nfunction _my_user_login($username,$password)\r\n{\r\n\t$creds = array();\r\n\t$creds['user_login'] = $username;\r\n\t$creds['user_password'] = $password;\r\n\t$creds['remember'] = true;\r\n\t$user = wp_signon( $creds, false );\r\n\twp_set_current_user($user-&gt;ID); //Here is where we update the global user variables\r\n\treturn $user;\r\n}\r\n\r\n//Commits a new post to the DB\r\nfunction _my_commit_post($postdata)\r\n{\r\n\tglobal $user_ID;\r\n\t$new_post = array(\r\n\t\t'post_title' =&gt; $postdata-&gt;title,\r\n\t\t'post_content' =&gt; $postdata-&gt;description,\r\n\t\t'post_status' =&gt; 'publish',\r\n\t\t'post_date' =&gt; date('Y-m-d H:i:s'),\r\n\t\t'post_author' =&gt; $user_ID,\r\n\t\t'post_type' =&gt; 'post',\r\n\t\t'post_category' =&gt; array(0),\r\n\t\t'tags_input' =&gt; $postdata-&gt;tags\r\n\t);\r\n\t$post_id = wp_insert_post($new_post,true);\r\n}\r\n\r\n//Print info about the user who is now logged in\r\nfunction _my_current_user()\r\n{\r\n\tglobal $current_user,$user_ID;\r\n\r\n\tif(is_user_logged_in())\r\n\t{\r\n\t\techo '&lt;br /&gt;User Logged in ok&lt;br /&gt;';\r\n\t\techo 'User ID is: '.$user_ID.'&lt;br /&gt;';\r\n\t\techo 'User login is: '.$current_user-&gt;user_login.'&lt;br /&gt;';\r\n\t}\r\n\telse\r\n\t\techo 'No user is logged in&lt; br/&gt;';\r\n}\r\n\r\n?&gt;</p></div>\n\";}"
categories:
    - Programming
tags:
    - $current_user
    - 'create user'
    - is_user_logged_in
    - php
    - wordpress
    - wp_signon
---

**Happy Christmas and holidays!**

I would be out in the sun if there was any! But since it’s raining I’m doing a bit of WordPress development for a project I’m working on.

The [*wp\_signon()*](http://codex.wordpress.org/Function_Reference/wp_signon) function logs a user in but for some reason after doing so the global user variables such as *$current\_user* and *$user\_ID* are not populated until the page is refreshed and calling *get\_currentuserinfo()* doesn’t populate them. The *is\_user\_logged\_in()* function also returns false. I think the problem has something to do with the cookie authentication process.

There is however a solution to the problem and it doesn’t involve refreshing the page. When you call *wp\_signon()* it returns a user object if the sign-on was successful, so we do have all the user info we need, it’s just not available in the standard global way. To fix this we just need to call the *[wp\_set\_current\_user()](http://codex.wordpress.org/Function_Reference/wp_set_current_user)* specifying the id of user object which *wp\_signon()* returned.

To demonstrate, here’s an example of creating a user, logging them in and submitting a post all in one page refresh.

```
<pre escaped="true" lang="php"><?php
_my_newuser('joe','itsasecret','joe@joe.com','Joe');

_my_user_login('joe','itsasecret');

_my_commit_post($newpost);

_my_current_user();

//Register a new user
function _my_newuser($username,$password,$email,$nickname)
{
	global $wpdb;
	$user_login = $wpdb->escape( $username );
	$user_pass = $password;
	$nickname = $nickname;
	$user_email = $username;
	$userdata = compact('user_login', 'user_email', 'user_pass', 'first_name','nickname' );
	return wp_insert_user($userdata);
}

//Log a user in and set them as current user
function _my_user_login($username,$password)
{
	$creds = array();
	$creds['user_login'] = $username;
	$creds['user_password'] = $password;
	$creds['remember'] = true;
	$user = wp_signon( $creds, false );
	wp_set_current_user($user->ID); //Here is where we update the global user variables
	return $user;
}

//Commits a new post to the DB
function _my_commit_post($postdata)
{
	global $user_ID;
	$new_post = array(
		'post_title' => $postdata->title,
		'post_content' => $postdata->description,
		'post_status' => 'publish',
		'post_date' => date('Y-m-d H:i:s'),
		'post_author' => $user_ID,
		'post_type' => 'post',
		'post_category' => array(0),
		'tags_input' => $postdata->tags
	);
	$post_id = wp_insert_post($new_post,true);
}

//Print info about the user who is now logged in
function _my_current_user()
{
	global $current_user,$user_ID;

	if(is_user_logged_in())
	{
		echo '<br />User Logged in ok<br />';
		echo 'User ID is: '.$user_ID.'<br />';
		echo 'User login is: '.$current_user->user_login.'<br />';
	}
	else
		echo 'No user is logged in< br/>';
}

?>
```

Of Course you might want some error handling in there! 😉 And did you notice the new syntax highlighting plug-in? Neat ay?