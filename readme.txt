=== Voce Group Keys ===
Contributors: banderon
Tags: cache, group, keys
Requires at least: 2.8
Tested up to: 4.0
Stable tag: 1.0.0
License: GPLv2 or later
License URI: http://www.gnu.org/licenses/gpl-2.0.html

Create string keys for caching based off of specified groups, allowing clearing keys based on the groups to which they
belong.

== Description ==

Gives functions to use to generate strings to use as keys for caching. Keys are generated based on a passed in name and
zero or more groups. Providing the same name and groups will return the same key until the entire cache is cleared
or the cache for a specific group name is cleared.

== Installation ==

1. Upload `voce-group-keys.php` to the `/wp-content/plugins/` directory
2. Activate the plugin through the 'Plugins' menu in WordPress
3. Use the plugin's functions where necessary in your code, making sure to check if the plugin is enabled

== Usage ==

$key: STRING
$groups: STRING|ARRAY

`<?php
if ( class_exists( 'Voce_Group_Keys' ) ) {

	// Get a key for $key that is in the $groups group(s)
	voce_get_cache_key( $key, $groups );

	// Clear the keys for $groups
	// If multiple groups are specified, keys in any of the specified groups will be cleared
	voce_clear_group_cache( $groups );

	// Clear all keys
	voce_clear_all_group_cache();
}`

= Example 1 =

`<?php
if ( class_exists( 'Voce_Group_Keys' ) ) {

	// Get a key without a group
	echo voce_get_cache_key( 'data' );  // data_4942b011eb

	// Get keys in a single group
	echo voce_get_cache_key( 'data', 'people' );  // data_9915443f5c
	echo voce_get_cache_key( 'more-data', 'people' );  // more-data_9915443f5c

	// The same key will be returned
	echo voce_get_cache_key( 'data', 'people' );  // data_9915443f5c

	// Clear keys in the 'people' group
	voce_clear_group_cache( 'people' );

	// After clearing keys, a new key is returned for keys using that group
	echo voce_get_cache_key( 'data', 'people' );  // data_77j18e728
	echo voce_get_cache_key( 'data' );  // data_4942b011eb

}`

= Example 2 =

`<?php
if ( class_exists( 'Voce_Group_Keys' ) ) {

	// Set transients using multiple groups
	echo voce_get_cache_key( 'user-data', 'users' );  // user-data_9915443f5c
	echo voce_get_cache_key( 'post-data', 'posts' );  // post-data_85fb002156
	echo voce_get_cache_key( 'user-post-data', array( 'posts', 'users' ) );  // user-post-data_4aee2c2c89

	// Clear any keys using the 'posts' group
	voce_clear_group_cache( 'posts' );

	// New keys returned for anything in the 'posts' group
	echo voce_get_cache_key( 'user-data', 'users' );  // user-data_9915443f5c
	echo voce_get_cache_key( 'post-data', 'posts' );  // post-data_820dd0dfb0
	echo voce_get_cache_key( 'user-post-data', array( 'posts', 'users' ) );  // user-post-data_b7ac93f802

	// The order of groups is irrelevant
	echo voce_get_cache_key( 'user-post-data', array( 'users', 'posts' ) );  // user-post-data_b7ac93f802

	// Clear any keys in either the 'users' or 'posts' groups
	voce_clear_group_cache( array( 'users', 'posts' ) );

	// Clear all keys
	voce_clear_all_group_cache();
}`

== Changelog ==

= 1.0.0 =
* Initial release
