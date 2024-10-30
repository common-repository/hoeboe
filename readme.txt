=== Hoeboe ===
Contributors: twicetwomedia
Donate link: https://twicetwomedia.com/wordpress-plugins/
Tags: transients, api, cache, caching, ajax, page speed, page load, site speed
License: GPLv3
License URI: http://www.gnu.org/licenses/gpl-3.0.html
Requires at least: 3.5
Tested up to: 5.4
Requires PHP: 5.3
Stable tag: 0.1.4

Easily update WordPress transients in the background via AJAX to increase site speed and avoid long page load times. Hoeboe can be especially helpful with caching of large external API calls or heavy internal database queries.

== Description ==

Easily update WordPress transients in the background via AJAX to increase site speed and avoid long page load times. Hoeboe can be especially helpful with caching of large external API calls or heavy internal database queries.

If you've used the WordPress Transients API, you already know how useful it can be with caching, page load, and site speed. If you've used transients to store data from external API calls or from heavy internal database queries, then you also know a few of its limitations. Namely, page load can be negatively impacted on the user session where a large transient gets updated.

Hoeboe helps to solve this problem of the one-off user who has to deal with potentially long page load while your site refreshes a transient in the background. With Hoeboe, you can choose to update those large transients in the background via AJAX. Your users won't notice anything different - other than possibly faster overall site speed.

== Installation ==

Upload the Hoeboe plugin and activate Hoeboe within wp-admin settings. You'll then be ready to upgrade your WordPress transients.

See the example below detailing how to use Hoeboe in your theme:

**Basic Implementation of a Transient**
`
<?php
//WP_Query function to be used to get data
function my_function_to_get_featured_posts($category, $posts_per_page) {
	$posts = new WP_Query(
	  array(
			"category" => $category,
			"posts_per_page" => $posts_per_page
	  )
	);
	return $posts;
}

//Attempt to get transient
$transient_name = "foo_featured_posts";
$featured = get_transient( $transient_name );

//Check for transient. If none, then execute WP_Query function
if ( false === ( $featured ) ) {
	
	$category = "featured";
	$posts_per_page = "5";
	$featured = my_function_to_get_featured_posts($category, $posts_per_page);

	//Put the results of the query in a transient. Expire after 12 hours.
	$expiration_time = 12 * HOUR_IN_SECONDS;
	set_transient( "foo_featured_posts", $featured, $expiration_time );
} 
?>
`

**Using Hoeboe with that same Transient outlined above**
`
<?php
//WP_Query function to be used to get data
function my_function_to_get_featured_posts($category, $posts_per_page) {
	$posts = new WP_Query(
	  array(
			"category" => $category,
			"posts_per_page" => $posts_per_page
	  )
	);
	return $posts;
}

$transient_name = "foo_featured_posts";
$my_function_name = 'my_function_to_get_featured_posts';
$category = "featured";
$posts_per_page = "5";
$my_function_parameters = array($category, $posts_per_page);
$transient_expire = 60;
$expiration_time = 12 * HOUR_IN_SECONDS;

if (class_exists('Hoeboe')) {
	$hoeboe = new Hoeboe();
	$transient_value = $hoeboe->hoeboe__updatetransient($transient_name, $my_function_name, $my_function_parameters, $expiration_time);
}
?>
`

= Links =

* [Detailed installation and implementation guide](https://twicetwomedia.com/wordpress-plugins/).

== Frequently Asked Questions ==

= Does Hoeboe replace WordPress transients? =

**No.** Hoeboe works alongside the Transients API to help you cache data better and, ultimately, provide a better user experience for your site visitors.

= If I activate Hoeboe, will it automatically affect all current WordPress transients that I use? =

**No.** You must implement Hoeboe individually within your theme even after the Hoeboe plugin is activated. Hoeboe is set up this way so that you can pick and choose which transients to use Hoeboe on. 

= How do I implement Hoeboe on a transient? =

**Only a couple of simple steps.** Visit the link below for more information. 

[Detailed installation and implementation guide](https://twicetwomedia.com/wordpress-plugins/).

== Changelog ==

= 0.1.4 =
*Release Date - Mar 31, 2020*
* Include new file

= 0.1.3 =
*Release Date - Mar 31, 2020*
* Only include jQuery if it is not already registered
* Improve admin settings

= 0.1.2 =
*Release Date - Jul 23, 2019*
* Fix PHP warnings on activation / deactivation

= 0.1.1 =
*Release Date - Dec 9, 2018*
* Tested up to WordPress 5.0
* Enqueue jQuery if not already present
* Better handle multiple Hoeboe/Transient calls on one page
* Better handle multiple Hoeboe/Transient requests in a short time frame

= 0.1.0 =
*Release Date - Dec 1, 2018*
* Initial release