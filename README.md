# third-description-in-product-page-in-woocommerce
third description in product page in woocommerce
add_filter( 'woocommerce_product_export_column_headers', 'export_third_desc_column' );
function export_third_desc_column( $headers ) {
    $headers['third_desc'] = 'Third Description';
    return $headers;
}
add_filter( 'woocommerce_product_export_custom_column', 'export_third_desc_column_data', 10, 2 );
function export_third_desc_column_data( $column_data, $product ) {
    $third_desc = get_post_meta( $product->get_id(), '_third_desc', true );
    if ( ! empty( $third_desc ) ) {
        $column_data['third_desc'] = $third_desc;
    } else {
        $column_data['third_desc'] = '';
    }
    return $column_data;
}


add_filter( 'woocommerce_csv_product_import_mapping_options', 'import_third_desc_column', 10, 1 );

function import_third_desc_column( $options ) {
    $options['Third Description'] = 'third_desc';
    return $options;
}

add_action( 'woocommerce_csv_product_imported', 'import_third_desc_column_data', 10, 3 );

function import_third_desc_column_data( $product_id, $product_data, $csv_data ) {
    $third_desc = isset( $csv_data['third_desc'] ) ? sanitize_text_field( $csv_data['third_desc'] ) : '';
    update_post_meta( $product_id, '_third_desc', $third_desc );
}
