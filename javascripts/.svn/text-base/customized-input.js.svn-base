var BrowserDetect = {
    init: function() {
        this.browser = this.searchString(this.dataBrowser) || "An unknown browser";
        this.version = this.searchVersion(navigator.userAgent)
            || this.searchVersion(navigator.appVersion)
            || "an unknown version";
        this.OS = this.searchString(this.dataOS) || "an unknown OS";
    },
    searchString: function(data) {
        for (var i = 0; i < data.length; i++) {
            var dataString = data[i].string;
            var dataProp = data[i].prop;
            this.versionSearchString = data[i].versionSearch || data[i].identity;
            if (dataString) {
                if (dataString.indexOf(data[i].subString) != -1)
                    return data[i].identity;
            } else if (dataProp)
                return data[i].identity;
        }
    },
    searchVersion: function(dataString) {
        var index = dataString.indexOf(this.versionSearchString);
        if (index == -1) return;
        return parseFloat(dataString.substring(index + this.versionSearchString.length + 1));
    },
    dataBrowser: [ 	{ string: navigator.userAgent, subString: "Chrome", identity: "Chrome" },
        { string: navigator.userAgent, subString: "OmniWeb", versionSearch: "OmniWeb/", identity: "OmniWeb" },
        { string: navigator.vendor, subString: "Apple", identity: "Safari", versionSearch: "Version" },
        { prop: window.opera, identity: "Opera" },
        { string: navigator.vendor, subString: "iCab", identity: "iCab" },
        { string: navigator.vendor, subString: "KDE", identity: "Konqueror" },
        { string: navigator.userAgent, subString: "Firefox", identity: "Firefox" },
        { string: navigator.vendor, subString: "Camino", identity: "Camino" },
        { string: navigator.userAgent, subString: "Netscape", identity: "Netscape" },
        { string: navigator.userAgent, subString: "MSIE", identity: "Explorer", versionSearch: "MSIE" },
        { string: navigator.userAgent, subString: "Gecko", identity: "Mozilla", versionSearch: "rv" },
        { string: navigator.userAgent, subString: "Mozilla", identity: "Netscape", versionSearch: "Mozilla" } ],
    dataOS: [ 	{ string: navigator.platform, subString: "Win", identity: "Windows" },
        { string: navigator.platform, subString: "Mac", identity: "Mac" },
        { string: navigator.userAgent, subString: "iPhone", identity: "iPhone/iPod" },
        { string: navigator.platform, subString: "Linux", identity: "Linux" } ]
};

BrowserDetect.init();

var Dropdown = {
	z_index_start: 100,
    init: function() {
        $('select.styled').each(function() {
            var id = "";
            var default_value = "";
            var selected_value = "";
            var default_label = "";
            var selected_label = "";
            var original_dropdown = "";
			
            original_dropdown = this;
			
            if ($(this).attr('id')) {
                id = "dropdown_"+$(this).attr('id');
            } else {
                $(this).attr('id',(new Date().getTime()*Math.floor((Math.random()*100)+1)).toString());
                id = "dropdown_"+$(this).attr('id');
            }
			
            selected_value = $(this).children('option:selected').val();
            selected_label = $(this).children('option:selected').text();
			
            if(selected_value == "") selected_value = default_value;
            if(selected_label == "") selected_label = default_label;
			
            $(this).children().each(function(){
                if($(this).val()=="-1"){
                    default_value = $(this).val();
                    default_label = $(this).text();
                }
            });
			
            $('<div class="customized-dropdown" id="'+id+'">').insertBefore(this);
            $('#'+id).append('<span class="default-value selected" value="'+selected_value+'">'+selected_label+'</span>');
            if( default_value == selected_value && default_label == selected_label ) {
                $('#'+id+' .default-value').removeClass('selected');
			}
			
            $('#'+id).append('<div class="options-content">');
            $('#'+id+' div.options-content').append('<div class="options-content-top"></div>');
            $('#'+id+' div.options-content').append('<div class="options-content-middle"></div>');
            $('#'+id+' div.options-content').append('<div class="options-content-bottom"></div>');
            $('#'+id+' div.options-content .options-content-middle').append('<label class="default-value">'+default_label+'</label>');
            $('#'+id+' div.options-content .options-content-middle').append('<ul class="options"></ul>');
            $('#'+id+' div.options-content').css('z-index',Dropdown.z_index_start);
			if(Dropdown.z_index_start!=0)
				Dropdown.z_index_start--;
			
			var counter = 1;
			$(this).children().each(function(){
                if($(this).val()!="-1"){	
                    default_value = $(this).val();
                    default_label = $(this).text();
                    $('#'+id+' ul.options').append('<li class=""><label id="'+counter.toString()+'_'+id+'" value="'+default_value+'">'+default_label+'</label></li>');
					counter++;
				}
            });
			
			$('#'+id+' .options').children().each(function(){
				if( $(this).find('label').attr('value') == selected_value ) {
					$(this).attr('selected','selected');
					$(this).addClass('selected');
				}
			});
				
            $('#'+id).click(function(e){
				var value_selected = false;
				var flag = false;
				
				$('.options-content').hide();
                $('span.default-value').show();
				
				if($('#'+id+' span.default-value').hasClass('selected')){ value_selected = true; }
				
                $('#'+id+' .options-content').show();
                $('#'+id+' span.default-value').hide();
                if ( value_selected ) {
					$('#'+id+' .options').children().each(function(){
						if($(this).attr('selected')=="selected"){
							var offset = $(this).position().top;
							var upperBound = $('#'+id+' .options').scrollTop();
							var lowerBound = $('#'+id+' .options').height();
							$("#"+id+" .options").scrollTop(upperBound+(offset-lowerBound)+10);
						}
					});
				} else {
					$('#'+id+' ul.options li').each(function(){
						if (flag) {
							$(this).removeClass('selected');
							$(this).removeAttr('selected');
						} else {
							$(this).addClass('selected');
							$(this).attr('selected','selected');
							flag = true;
						}	
					});
					$('#'+id+' ul.options').scrollTop(0);
				}
				
				Dropdown.timer = new Date().getTime();
				$(document).unbind('keydown').bind('keydown', function(e){
					Dropdown.key_handler(e,document.getElementById(id));
				});
				
				e.stopPropagation();
            });
            $('#'+id+' ul.options li').click(function(e){
                $('#'+id+' span.default-value').text($(this).children('label').text());
                $('#'+id+' span.default-value').addClass('selected');
                $('#'+id+' span.default-value').attr('value',$(this).children('label').attr('value'));
                $('#'+id+' .options-content').hide();
                $('#'+id+' span.default-value').show();
                $(original_dropdown).val($(this).children('label').attr('value'));
                e.stopPropagation();
            });
			$('#'+id+' ul.options li').mouseover(function(e){
				$('#'+id+' ul.options li').each(function(){
					$(this).removeClass('selected');
					$(this).removeAttr('selected');
				});
				$(this).addClass('selected');
				$(this).attr('selected','selected');
			});
            $(document).click(function() {
                $('.options-content').hide();
                $('span.default-value').show();
				$(document).unbind('keydown');
            });
            $(this).hide();
        });
    },
	search_word: '',
    timer: 0,
	key_handler: function(key, element) {
		var search_word_length, i = 0;
        var search_array = new Array();
        var search_string;
		// Key handling for all alphabets A-Z
		if(key.keyCode>=65 && key.keyCode <= 90){
            timeOfOccurence = new Date().getTime();
            search_string = String.fromCharCode(key.which).toLowerCase();
            if( timeOfOccurence-this.timer < 1000 ) { this.search_word += search_string; } else { this.search_word=search_string; }
            this.timer = timeOfOccurence;
			search_word_length = this.search_word.length;
            
			$("#"+element.id+" .options").children().each(function() {
				str = $(this).find('label').text().toLowerCase();
                find = str.substring(0, search_word_length).toLowerCase();
                if( Dropdown.search_word == find ){ search_array[i++] = $(this).find('label').attr('id').split("_")[0]; }
            });
			
            if(search_array.length!=0) {
                var oneWasSelected = -1;
                var e = 0;
                for(e = 0; e < search_array.length; e++){
                    $("#"+element.id+" .options").children().each(function(){
                        if($(this).find('label').attr('id').split("_")[0]==search_array[e]){
                            if($(this).attr('selected')=='selected'){
                                oneWasSelected = e;
                                return false;
                            }
                        }
                    });
                }
                if(oneWasSelected!=-1) {
                    $("#"+element.id+" .options").children().each(function(){
                        if($(this).find('label').attr('id').split("_")[0]==search_array[oneWasSelected]){
                            $(this).removeAttr('selected').removeClass('selected');
                        }
                    });
                    if(oneWasSelected < search_array.length-1) {
                        $("#"+element.id+" .options").children().each(function(){
                            if($(this).find('label').attr('id').split("_")[0]==search_array[oneWasSelected+1]){
                                $(this).attr('selected','selected').addClass('selected');
                            }
                        });
                        e = oneWasSelected +1;
                    } else {
                        $("#"+element.id+" .options").children().each(function(){
                            if($(this).find('label').attr('id').split("_")[0]==search_array[0]){
                                $(this).attr('selected','selected').addClass('selected');
                            }
                        });
                        e = 0;
                    }
                } else {
                    $("#"+element.id+" .options").children().each(function(){
                        if($(this).attr('selected')=='selected'){
                            $(this).removeAttr('selected').removeClass('selected');
                        }
                    });
                    $("#"+element.id+" .options").children().each(function(){
                        if($(this).find('label').attr('id').split("_")[0] == search_array[0]){
                            $(this).addClass('selected').attr('selected','selected');
                            return false;
                        }
                    });
                    e = 0;
                }
                var offset;
                $("#"+element.id+" .options").children().each(function(){
                    if($(this).find('label').attr('id').split("_")[0]==search_array[e]){
                        offset = $(this).position().top;
                    }
                });
                var upperBound = $('#'+element.id+' .options').scrollTop();
                var elementHeight = $('#'+element.id+' .options li').height()+4;
                var lowerBound = $('#'+element.id+' .options').height();
                if(offset+elementHeight>lowerBound) {
                    $("#"+element.id+" .options").animate({
                        scrollTop: upperBound+(offset-lowerBound)+10
                    }, 1000);
                } else if (offset<30) {
                    $("#"+element.id+" .options").animate({
                        scrollTop: upperBound+(offset)-30
                    }, 1000);
                }
            }
        } else if (key.keyCode == 40){
			// Key handling of the down arrow
			$("#"+element.id+" .options").children().each(function(){
                if($(this).attr('selected')=='selected'){
                    $(this).removeAttr('selected').removeClass('selected');
                    if( $(this).children().attr('id').split("_")[0] < $("#"+element.id+" .options li").length ){
                        if ( $(this).next() ) {
                            var activeItem, upperBound, lowerBound, offsetTop;
                            var elementHeight = $('#'+element.id+' .options li').height()+4;
                            $(this).next().attr('selected', 'selected').addClass('selected');
                            activeItem = $(this).next();
                            upperBound = $('#'+element.id+' .options').scrollTop();
                            offsetTop = $(this).next().position().top;
                            lowerBound = $('#'+element.id+' .options').height();
                            if ( offsetTop + elementHeight > lowerBound ){ $('#'+element.id+' .options').scrollTop( upperBound + ( offsetTop - lowerBound ) ); }
                            key.stopImmediatePropagation();
                            key.preventDefault();
                            return false;
                        }
                        
                    } else {
                        $(this).attr('selected', 'selected').addClass('selected');
                        key.stopImmediatePropagation();
                        key.preventDefault();
                        return false;
                    }
                }
            });
        } else if(key.keyCode == 38){
            // Key handling of the up arrow
            $("#"+element.id+" .options").children().each(function(){
                if($(this).attr('selected')=='selected'){
                    $(this).removeAttr('selected').removeClass('selected');
                    if($(this).children().attr('id').split("_")[0]>"1"){
                        if($(this).prev()){
                            var activeItem, upperBound, lowerBound, offsetTop;
                            var elementHeight = $('#'+element.id+' .options li').height()+4;
                            $(this).prev().attr('selected', 'selected').addClass('selected');
                            activeItem=$(this).prev();
                            upperBound = $('#'+element.id+' .options').scrollTop();
                            offsetTop = $(this).prev().position().top;
                            lowerBound = $('#'+element.id+' .options').height();
                            if ( offsetTop < 30 ) { $('#'+element.id+' .options').scrollTop(upperBound+offsetTop-lowerBound); }
                            key.stopImmediatePropagation();
                            key.preventDefault();
                            return false;
                        }
                    } else {
                        $(this).attr('selected', 'selected').addClass('selected');
                        key.stopImmediatePropagation();
                        key.preventDefault();
                        return false;
                    }
                }
            });
        } else if ( key.keyCode == 9 ) {
			// Key handling for tab
		} else if ( key.keyCode == 13 ) {
			// Key handling for enter
			$('#'+element.id+' .options').children().each(function(){
                if($(this).attr('selected')=="selected"){
                    $(this).find('label').click();
                }
            });
		}
	}
};

var Textbox = {
    init: function() {
        $('input[type=text].styled,input[type=password].styled').each(function() {
            var placeholder = "";
            var hidden_element = "";
            var id = "";
            var identifier = (new Date().getTime()*Math.floor((Math.random()*100)+1)).toString();
            var init_class = "";

            if ($(this).attr('placeholder')) {
                placeholder = $(this).attr('placeholder');
                $(this).removeAttr('placeholder');
            }

            if ($(this).attr('id')) {
                id = $(this).attr('id');
            } else {
                $(this).attr('id','inptxt_'+(new Date().getTime()*Math.floor((Math.random()*100)+1)).toString());
                id = $(this).attr('id');
            }

            if ($(this).val() == "") {
                $(this).val(placeholder);
                init_class = "input-text";
            } else {
                init_class = "active-input";
            }

            hidden_element = '<input type="hidden" value="' + placeholder + '" id="' + id + '_placeholder" />';
            $('<div class="customized-input ' + init_class + ' ' + identifier + '"></div>').insertBefore(this);
            $(this).appendTo('.' + identifier);
            $('.customized-input').removeClass(identifier);
            $(hidden_element).insertBefore(this);
            $('<div class="input-left">&nbsp;</div>').insertBefore($(this).prev());
            $('<div class="input-right">&nbsp;</div>').insertAfter(this);
        });
        $("body").on("focus", "input[type=text].styled", function(){ Textbox.focus_element(this); });
        $("body").on("blur", "input[type=text].styled", function(){ Textbox.blur_element(this); });
        $("body").on("keydown", "input[type=text].styled", function(e){ Textbox.type_element(this, e); });
        $("body").on("focus", "input[type=password].styled", function(){ Textbox.focus_element(this); });
        $("body").on("blur", "input[type=password].styled", function(){ Textbox.blur_element(this); });
        $("body").on("keydown", "input[type=password].styled", function(e){ Textbox.type_element(this, e); });
    },
    focus_element: function(el) {
        if ($(el).hasClass("password")) {
            if (el.type == "text") {
                Textbox.resetElement(el);
                $(el).parent().addClass("input-text-unactive");
                $(el).caretToStart();
            } else {
                $(el).parent().removeClass("input-text input-text-active input-text-unactive active-input input-text-error input-text-error-empty");
                $(el).parent().addClass("input-text-active");
            }
        } else {
            if ($(el).parent().hasClass("input-text-error")) {
                Textbox.resetElement(el);
                $(el).parent().addClass("input-text-error");
            } else if ($(el).parent().hasClass("input-text")) {
                Textbox.resetElement(el);
                $(el).parent().addClass("input-text-unactive");
                $(el).caretToStart();
            } else if ($("#" + el.id).parent().hasClass("input-text-error-empty")) {
                Textbox.resetElement(el);
                $(el).parent().addClass("input-text-unactive");
                $(el).caretToStart();
            } else {
                Textbox.resetElement(el);
                $(el).parent().addClass("input-text-active");
            }
        }
    },
    blur_element: function(el){
        var placeholder = "";
        var placeholder_id = el.id + "_placeholder";
		
        placeholder = $("#"+placeholder_id).val();
        if(placeholder == "undefined") placeholder = "";
		
        if ($(el).hasClass('password')) {
            if (el.value == "") {
                el = Textbox.changeInputType(el, "text", placeholder);
                Textbox.resetElement(el);
                $(el).parent().addClass("input-text");
            } else if ($(el).parent().hasClass("input-text-unactive")) {
                Textbox.resetElement(el);
                $(el).parent().addClass("input-text");
            } else if ($(el).parent().hasClass("input-text-error")) {
                Textbox.resetElement(el);
                $(el).parent().addClass("input-text-error");
            } else {
                Textbox.resetElement(el);
                $(el).parent().addClass("active-input");
            }
            return el;
        } else {
            if ($(el).parent().hasClass("input-text-error")) {
                Textbox.resetElement(el);
                $(el).parent().addClass("input-text-error");

            } else if ($(el).parent().hasClass("input-text-error-empty")) {
                Textbox.resetElement(el);
                $(el).parent().addClass("input-text-error-empty");
                el.value = placeholder;
            } else if ($(el).parent().hasClass("input-text")) {
                el.value = placeholder;
                Textbox.resetElement(el);
                $(el).parent().addClass("input-text");
            } else if ($(el).parent().hasClass("input-text-unactive")) {
                Textbox.resetElement(el);
                $(el).parent().addClass("input-text");
                el.value = placeholder;
            } else {
                if (el.value == "" || el.value == placeholder) {
                    el.value = placeholder;
                    Textbox.resetElement(el);
                    $(el).parent().addClass("input-text");
                } else {
                    Textbox.resetElement(el);
                    $(el).parent().addClass("active-input");
                }
            }
        }
    },
    type_element: function(el, event){
        if ($(el).parent().hasClass("input-text-unactive") || $(el).parent().hasClass("input-text-error-empty")) {
            if ($(el).hasClass('password')) {
                if (el.type == "text") {
                    el = Textbox.changeInputType(el, "password", "");
                    el.focus();
                    Textbox.resetElement(el);
                    $(el).parent().addClass("input-text-active");
                    $(el).caretToEnd();
                }
            } else {
                Textbox.resetElement(el);
                $(el).parent().addClass("input-text-active");
                $(el).val("");
            }
        }
    },
    resetElement: function(el){
        $(el).parent().removeClass("input-text input-text-unactive input-text-active active-input input-text-error input-text-error-empty");
    },
    errorElement: function(el){
        if ($(el).parent().hasClass("input-text") || $(el).parent().hasClass("input-text-unactive")) {
            Textbox.resetElement(el);
            $(el).parent().addClass("input-text-error-empty");
        } else if ($(el).parent().hasClass("input-text-active") || $(el).parent().hasClass("active-input")) {
            Textbox.resetElement(el);
            $(el).parent().addClass("input-text-error");
        }
    },
    changeInputType: function(oldObject, oType, value){
        newObject = document.createElement('input');
        newObject.type = oType;
        if (oldObject.size) newObject.size = oldObject.size;
        if (oldObject.name) newObject.name = oldObject.name;
        if (oldObject.id) newObject.id = oldObject.id;
        if (oldObject.className) newObject.className = oldObject.className;
        if (oldObject.onblur) newObject.onblur = oldObject.onblur;
        if (oldObject.onfocus) newObject.onfocus = oldObject.onfocus;
        if (oldObject.onkeyup) newObject.onkeyup = oldObject.onkeyup;
        if (oldObject.onkeydown) newObject.onkeydown = oldObject.onkeydown;
        if (oldObject.tabIndex) newObject.tabIndex = oldObject.tabIndex;
        if (oldObject.onkeypress) newObject.onkeypress = oldObject.onkeypress;
        if (oldObject.maxLength > 0) newObject.maxLength = oldObject.maxLength;
        newObject.value = value;
        oldObject.parentNode.replaceChild(newObject, oldObject);
        return newObject;
    }
};