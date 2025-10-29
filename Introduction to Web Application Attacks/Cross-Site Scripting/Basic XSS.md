# Basic XSS

Simple JavaScript Function
>``` shell
>function multiplyValues(x,y) {
>  return x * y;
>}
> 
>let a = multiplyValues(3, 5)
>console.log(a)
>```

Special characters for HTML and JavaScript
>``` shell
>< > ' " { } ;
>```

Inspecting Visitor Plugin Record Creation Function
>``` shell
>function VST_save_record() {
>	global $wpdb;
>	$table_name = $wpdb->prefix . 'VST_registros';
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

Inspecting Visitors Plugin Record Visualization Function
>``` shell
>$i=count(VST_get_records($date_start, $date_finish));
>foreach(VST_get_records($date_start, $date_finish) as $record) {
>    echo '
>        <tr class="active" >
>            <td scope="row" >'.$i.'</td>
>            <td scope="row" >'.date_format(date_create($record->datetime), get_option("links_updated_date_format")).'</td>
>            <td scope="row" >'.$record->patch.'</td>
>            <td scope="row" ><a href="https://www.geolocation.com/es?ip='.$record->ip.'#ipresult">'.$record->ip.'</a></td>
>            <td>'.$record->useragent.'</td>
>        </tr>';
>    $i--;
>}
>```
