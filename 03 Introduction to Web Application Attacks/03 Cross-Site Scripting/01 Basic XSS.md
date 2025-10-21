# Basic XSS

Special characters for HTML and JavaScript
>``` shell
>< > ' " { } ;
>```

Inspecting Visitor Plugin Record Creation Function
>``` php
>function VST_save_record() {
>	global $wpdb;
>	$table_name = $wpdb->prefix . 'VST_registros';>>
>
>	VST_create_table_records();
>
>	return $wpdb->insert(
>				$table_name,
>				array(
>					'patch' => $_SERVER["REQUEST_URI"],
>					'datetime' => current_time( 'mysql' ),
>					'useragent' => $_SERVER['HTTP_USER_AGENT'],
>					'ip' => $_SERVER['HTTP_X_FORWARDED_FOR']
>				)
>			);
>}
>```
