# HTML-Filter
Advanced HTML dropdown with ordering filters, search filters, selected items counter and Accept or Cancel buttons
<img src="https://i.imgur.com/stC61Xt.png">

You will need:

1. jQuery >= 3.0
2. HTML5
3. CSS3

<h1>Demo:</h1>

<html>
    <head>
        <meta charset="UTF-8">
        <title>Advanced Filter</title>
        <style>
            .dropdown-advanced {
                width: 50%;
                display: block;
                margin: 0 auto;
            }
            .dropdown-advanced ul {
                padding: 0px;
                margin: 0;
            }
            .dropdown-advanced .container-elements.closed {
                display: none;
            }
            .dropdown-advanced .container-buttons.closed {
                display: none;
            }
            .dropdown-advanced button {
                margin: 10px auto;
                padding: 10px;
                cursor: pointer;
            }
            .dropdown-advanced button.cancel {

            }
            .dropdown-advanced button.accept {

            }
            .dropdown-advanced label {
                cursor: pointer;
            }
            .dropdown-advanced li {
                list-style: none;
                padding: 0px;
                padding: 10px;
                margin: 0;
            }
            .dropdown-advanced .select-all-option-container {
                /*font-weight: bold;
                text-align: center;
                width: 100%;
                display: block;*/
            }
            .dropdown-advanced li:nth-child(even) {
                background-color: #f2f2f2;
                color: #000;
            }
            .dropdown-advanced li:nth-child(odd) {
                background-color: #fff;
                color: #000;
            }
            .dropdown-advanced .opened {
                border: 1px solid #f2f2f2;
            }
            .dropdown-advanced .options-selected {
                padding: 10px;
                border: 1px solid #f2f2f2;
                cursor: pointer;
                font-weight: bold;
            }
            .options-selected span {
                font-size: 12px;
            }
            .dropdown-advanced .search-button {
                width: 100%;
                padding: 10px;
                border: 1px solid #f2f2f2;
            }
            .dropdown-advanced .order-option {
                padding: 10px;
                border-bottom: 1px solid #f2f2f2;
                cursor: pointer;
            }
        </style>
    </head>
    <body>
        <div class="dropdown-advanced">
            <div class="options-selected dropdown-advanced-fire-action">
                Seleccionar opciones <img src="https://i.imgur.com/WiiLexh.png" alt=""> <span class=""></span>
            </div>
            <div class="closed container-elements">
                <input type="text" value="" placeholder="Buscar" class="search-button">
                <div class="order-option select-all-option-container">
                    <label><input class="select-all-option" type="checkbox">De/Seleccionar todos</label>
                </div>
                <div class="order-option re-order-filter" data-action="asc">
                    Ordenar de la A <img src="https://i.imgur.com/qjOEYLM.png" alt=""> Z
                </div>
                <div class="order-option re-order-filter" data-action="desc">
                    Ordenar de la Z <img src="https://i.imgur.com/qjOEYLM.png" alt=""> A
                </div>
                <ul>
                    <li><label><input data-id="0" class="item-option" value="Elemento uno" type="checkbox">Elemento uno</label></li>
                    <li><label><input data-id="1" class="item-option" value="Elemento dos" type="checkbox">Elemento dos</label></li>
                    <li><label><input data-id="2" class="item-option" value="Elemento tres" type="checkbox">Elemento tres</label></li>
                    <li><label><input data-id="3" class="item-option" value="Elemento cuatro" type="checkbox">Elemento cuatro</label></li>
                    <li><label><input data-id="4" class="item-option" value="Elemento das" type="checkbox">Elemento das</label></li>
                </ul>
            </div>
            <div class="container-buttons closed" align="center">
                <button class="accept">Aceptar</button>
                <button class="cancel">Cancelar</button>
            </div>
        </div>
        <script src="https://code.jquery.com/jquery-3.5.1.min.js" integrity="sha256-9/aliU8dGd2tb6OSsuzixeV4y/faTqgFtohetphbbj0=" crossorigin="anonymous"></script>
        <script>
            $(document).ready(function() {
                $("body").on("click", ".re-order-filter", function() {
                   $('.select-all-option').prop('checked', false);
                   let action = $(this).attr('data-action'),
                        list_elements = [];
                   $('.item-option').each(function(index) {
                        list_elements.push({
                            name : $(this).val(),
                            id : $(this).attr('data-id'),
                        });
                   });
                   if ( action == "asc" ) {
                       list_elements = list_elements.sort((a, b) => {
                           if (a.name < b.name) return -1
                           return a.name > b.name ? 1 : 0
                       });
                   }
                   else {
                       list_elements = list_elements.sort((a, b) => {
                           if (a.name < b.name) return -1
                           return a.name > b.name ? 1 : 0
                       }).reverse();
                   }
                   $('.container-elements ul').html('');
                   for ( let item = 0; item < list_elements.length; item++ ) {
                       $('.container-elements ul').append(function() {
                           let html = '';
                           html = '<li><label><input data-id="'+(list_elements[item].id)+'" class="item-option" value="'+(list_elements[item].name)+'" type="checkbox">'+(list_elements[item].name)+'</label></li>';
                           return html;
                       });
                   }
                });
                $("body").on("keyup", ".search-button", function() {
                    let search_content = $('.search-button').val().trim().toLowerCase();
                    if ( search_content.length > 0 ) {
                        $('.item-option').each(function(index) {
                            let content = $(this).val().toLowerCase(),
                                element = $(this);
                            element.parent('label').parent('li').hide();
                            if ( content.indexOf(search_content) >= 0 ) {
                                element.parent('label').parent('li').show();
                            }
                        });
                    }
                    else {
                        $('.item-option').parent('label').parent('li').show();
                    }
                });

                $("body").on("click", ".container-buttons .accept", function() {
                    $('.dropdown-advanced .container-elements').removeClass('opened');
                    $('.dropdown-advanced .container-buttons').removeClass('opened');

                    $('.dropdown-advanced .container-elements').addClass('closed');
                    $('.dropdown-advanced .container-buttons').addClass('closed');

                    $('.options-selected span').html('('+($('.item-option:checked').length)+') seleccionados');
                    $('.search-button').val("");
                    $('.item-option').parent('label').parent('li').show();
                });

                $("body").on("click", ".container-buttons .cancel", function() {
                    $('.dropdown-advanced .container-elements').removeClass('opened');
                    $('.dropdown-advanced .container-buttons').removeClass('opened');

                    $('.dropdown-advanced .container-elements').addClass('closed');
                    $('.dropdown-advanced .container-buttons').addClass('closed');
                    $('.search-button').val("");
                    $('.item-option').parent('label').parent('li').show();
                });

               $("body").on("change", ".select-all-option", function() {
                    if ( $(this).is(':checked') ) {
                        $('.item-option').prop('checked', true);
                    }
                    else {
                        $('.item-option').prop('checked', false);
                    }
               });
               $("body").on("click", ".dropdown-advanced-fire-action", function() {
                   if ( $('.dropdown-advanced .container-elements').hasClass('opened') ) {
                       $('.dropdown-advanced .container-elements').removeClass('opened');
                       $('.dropdown-advanced .container-buttons').removeClass('opened');

                       $('.dropdown-advanced .container-elements').addClass('closed');
                       $('.dropdown-advanced .container-buttons').addClass('closed');
                   }
                   else {
                       $('.dropdown-advanced .container-elements').addClass('opened');
                       $('.dropdown-advanced .container-buttons').addClass('opened');

                       $('.dropdown-advanced .container-elements').removeClass('closed');
                       $('.dropdown-advanced .container-buttons').removeClass('closed');
                   }

               });
            });
        </script>
    </body>
</html>

Enjoy it!
