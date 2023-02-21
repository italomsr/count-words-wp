## Contador de Palavras em Posts
Este plugin conta o número de palavras e caracteres de cada post e exibe na listagem de posts dentro da área admin em "/wp-admin/edit.php". Ele utiliza programação orientada a objetos (OOP) e consiste em uma única classe PHP chamada "Contador_Palavras_Posts".

##  O que o plugin faz?
O plugin adiciona uma nova coluna chamada "Contador de Palavras" à listagem de posts dentro da área admin em "/wp-admin/edit.php". Para cada post, ele exibe o número de palavras e caracteres presentes no conteúdo do post.

## O código completo

```<?php
/*
Plugin Name: Contador de Palavras
Plugin URI: https://github.com/italomsr/count-words-wp/
Description: Este plugin conta o número de palavras e caracteres de cada post e exibe na listagem de posts.
Version: 1.0
Author: Italo Mariano 
Author URI: https://www.linkedin.com/in/italomsr/
License: GPL2
*/

class Contador_Palavras_Posts {
    public function contar_palavras_caracteres($content) {
        $palavras = str_word_count(strip_tags($content));
        $caracteres = strlen(strip_tags($content));
        return array($palavras, $caracteres);
    }

    public function adicionar_colunas($columns) {
        $columns['contador_palavras'] = 'Count';
        return $columns;
    }

    public function exibir_colunas($column_name, $post_id) {
        if ($column_name === 'contador_palavras') {
            $contador_palavras = $this->contar_palavras_caracteres(get_the_content());
            $palavras = $contador_palavras[0];
            $caracteres = $contador_palavras[1];
            echo "Words: <b>" . $palavras . "</b><br>";
            echo "Characters: <b>" . $caracteres. "</b>";
        }
    }
}

$contador_palavras = new Contador_Palavras_Posts();
add_filter('manage_posts_columns', array($contador_palavras, 'adicionar_colunas'));
add_action('manage_posts_custom_column', array($contador_palavras, 'exibir_colunas'), 10, 2);
