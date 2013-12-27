defaultPlugins
==========================

Set of the most common used jQuery functionalities.

## List of functions ##

__tmMaxHeight__

* Calculating max height on multiple elements and add biggest height to all element

```javascript
$.fn.tmMaxHeight = function(){
  var maxHeight = 0;
  this.each(function(){
    // Set height to auto because of resize effect
    $(this).css('height', 'auto');
    maxHeight = maxHeight > $(this).height() ? maxHeight : $(this).height();
  });
  this.css( 'height', (parseInt(maxHeight) + 'px') );
};
```

Usage:
```javascript
$(function(){
  $('ul li').tmMaxHeight();
});
```

__tmMaxHeightResize__

* Calculating max height on multiple elements and add biggest height to all element including on resize window. In order to work, default tmMaxHeight function must be copied

```javascript
// Default tmMaxHeight
$.fn.tmMaxHeight = function(){
  var maxHeight = 0;
  this.each(function(){
    // Set height to auto because of resize effect
    $(this).css('height', 'auto');
    maxHeight = maxHeight > $(this).height() ? maxHeight : $(this).height();
  });
  this.css( 'height', (parseInt(maxHeight) + 'px') );
};
// Resize tmMaxHeightResize
$.fn.tmMaxHeightResize = function(){
  var that = this;
  this.tmMaxHeight();
  $(window).resize(function(){
    that.tmMaxHeight();
  });
};
```

Usage:
```javascript
$(function(){
  $('ul li').tmMaxHeightResize();
});
```

__linkify__

* Setting entire selected html element as an anchor. This trick is fixing HTML validation (img tag inside anchor)

```javascript
$.fn.linkify = function() {
  return this.each(function(){
    var link = $(this).find("a:first");
    var all_links = $(this).find('a')
    all_links.click(function(e){
      e.preventDefault();
    });
    $(this).click(function(e){
      var video = $(this).find('video.sublimed');
      if(video.length){
        sublimevideo.play(video[0]);
      }else{
        link.length && (window.location.href = link.attr('href'));
      }
    });
    if(link.length){$(this).addClass("linkify");}
  });
};
```

Usage:
```javascript
$(function(){
  $('ul li').linkify();
});
```

__tmAddChildClass__

* Adding classes to list of elements. Best usage is for first/last child in the list of elements.
* If no position is specified it will return first child only.
* If any position beside first or last is passed it will be applied to all passed elements.

```javascript
$.fn.tmAddChildClass = function(position, className){
  position = position || 'first';
  className = className || 'tmClass'
  if( position == 'first' ){
    this.first().addClass( position+'-child' );
  }else if( position == 'last' ){
    this.last().addClass( position+'-child' );
  }else{
    this.addClass(position);
  }
};
```

Usage:
```javascript
$(function(){
  // Default
  $('ul li').tmAddChildClass();
  // First child
  $('ul li').tmAddChildClass('first');
  // Last child
  $('ul li').tmAddChildClass('last');
});
```

__clickableTables__

* Link whole table row or table data
* If there is just one link inside whole tr, set that link on the whole tr
* If there are more than one links inside tr, iterate through each td:
*  - If td has one link inside, put an link to it
*  - If td has more than one link, do nothing ( keep default behaviour )
* Default CSS class is "linkable", but you can pass another one
* Do not forget to modify your CSS styles

```javascript
$.fn.tableLinks = function(opt = {}){
  var that = this,
      tableBody = $(this).find("tbody").length ? true : false,
      query = tableBody ? "tbody tr" : "tr:not(:first-child)",
      rowClass = opt.rowClass || "linkable",
      preventElements = ["input", "textarea", "select", "option"];

  $(this).find(query).each(function(){
    if( $(this).find("a").length === 1 ){
      attachEventListener( $(this), "tr" );
    }else if( $(this).find("a").length > 1 ){
      $(this).find("td").each(function(){
        if( $(this).find("a").length && $(this).find("a").length === 1 ) attachEventListener( $(this), "td" );
      });
    }
  });

  function attachEventListener(el, selector){
    createStyle( el );
    el.on("click", $(selector), function(e){
      if( $.inArray( e.target.localName, preventElements ) !== -1 ){
        e.stopPropagation();
        return;
      }
      redirect( $(this) );
    });
  };

  function redirect(el){
    var href = el.find("a").attr("href");
    if( el.closest("tr").hasClass(rowClass) ){
      href = el.closest("tr").find("a").attr("href");
    }
    if( el.attr("target") == "_blank" ) {
      window.open( href, "" );
    }else{
      window.location.href = href;
    }
  };

  function createStyle(el){
    el.addClass( rowClass );
  };

};
```

Usage:
```javascript
$(function(){
  $("table.table").tableLinks({
    rowClass: "linkable"
  });
});
```

## Author ##
Tomislav Matijević
Marko Žabčić (linkify)

