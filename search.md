---
title: Search Results
hide_search: true
redirect_from: 
- /index.php/Special:SearchPathways
---

<script async src="https://cse.google.com/cse.js?cx=c1b9a23fc5f2875e3">
</script>

<span style="font-size: 0.7rem;padding-left: 13px;margin: 32px 0px 0px 13px;position: absolute;float:left;">
            <a href="/browse/table.html">Advanced search</a></span>
<div id="myResults" class="gcse-searchresults-only"></div>

<script>
const myInitCallback = function() {
  if (document.readyState == 'complete') {
    // Document is ready when Search Element is initialized.
    // Render an element with both search box and search results in div with id 'myResults'.
    google.search.cse.element.render(
        {
          div: "myResults",
          tag: 'search'
         });
  } else {
    // Document is not ready yet, when Search Element is initialized.
    google.setOnLoadCallback(function() {
        if (!document.getElementById('___gcse_0')) { // HACK to prevent duplicate result divs
       // Render an element with both search box and search results in div with id 'myResults'.
        google.search.cse.element.render(
            {
              div: "myResults",
              tag: 'search'
            });
        }
    }, true);
  }
  document.getElementById('advsearch').innerHTML = "";
};

const makeTwoPartCallback = () => {
  let saveForRenderCallback;
  const readyCallback = (name, q, promos, results, resultsDiv) =>
  {
    saveForRenderCallback = [];
    for (const result of results) {
        // console.log(result);
        if(result['richSnippet']){
          saveForRenderCallback.push(
            {myTitle: result['richSnippet']['metatags']['citationTitle'],
            myWpid: result['id'],
            myOrganisms: result['organisms'],
            myDescription: result['richSnippet']['metatags']['citationAbstract'],
            myFirstAuthor: result['richSnippet']['metatags']['citationAuthor'],
            myFirstAuthorOrcid: result['richSnippet']['metatags']['citationAuthorOrcid'],
            myUrl: result['richSnippet']['metatags']['citationFulltextHtmlUrl'],
            myEdited: result['richSnippet']['metatags']['citationPublicationDate'],
            myGoogleUrl: result['url']}
          );
          if(result['richSnippet']['thumbnail']){
            saveForRenderCallback[saveForRenderCallback.length-1].myThumb =  result['richSnippet']['thumbnail']['src']
          }
          continue;
        }
      saveForRenderCallback.push({});
    }
  };
  const renderedCallback = (name, q, promos, results) => {
    for (let i = 0; i < results.length; ++i) {
    //   console.log(saveForRenderCallback[i]);
      myContent = "";
      const div = results[i];
      const title = saveForRenderCallback[i]['myTitle'];
      if (typeof title == 'undefined') {
        continue;
      }
      const wpid = saveForRenderCallback[i]['myWpid'];
      const orgs = saveForRenderCallback[i]['myOrganisms'];
      desc = saveForRenderCallback[i]['myDescription'];
      if (typeof desc !== 'undefined') {
        if (desc.length > 455) {
            desc = desc.slice(0, 450) + '...';
        }
      } else {
        desc = "No description";
      }
      const url = saveForRenderCallback[i]['myUrl'];
      const gurl = saveForRenderCallback[i]['myGoogleUrl'];
      const fauthor = saveForRenderCallback[i]['myFirstAuthor'];
      const faorcid = saveForRenderCallback[i]['myFirstAuthorOrcid'];
      const edited = saveForRenderCallback[i]['myEdited'];
      thumb = saveForRenderCallback[i]['myThumb'];
      if (typeof thumb !== 'undefined') {
        // console.log("myThumb: " + thumb);
        const badwpid = "/{ page.wpid }}-";
        if (thumb.includes(badwpid)) {
            const startIndex = thumb.indexOf('/WP') + 1;
            const endIndex = thumb.indexOf('/', startIndex);
            const goodwpid = '/'+ thumb.substring(startIndex, endIndex) + '-';
            thumb = thumb.split(badwpid).join(goodwpid);
        }
      } else {
        thumb = "/assets/img/missing-image.png";
      }
       myContent = 
         '<a style="text-decoration:none;" href="'+gurl+'"><table style="border-style:none; padding:0px; margin:0px;"><tr><td style="width:160px;border-style:none;"><img alt="Pathway thumbnail" ' +
         'src="'+ thumb +'"/></td>'; 
        myContent += 
         '<td style="border-style:none;"><a style="font-size:16px;text-decoration:none;color:#1A0DAB;" href="'+gurl+'">' + title + '</a>' +
         '<br/><span style="color:#777777;">' + url;
        if (typeof orgs !== 'undefined') {
            myContent += ' - ' + orgs;
        }
        myContent += 
        '</span>' +
        '<br/><span style="font-size:13px;">' + desc + '</span>' +
        '<br/><span style="color:#777777;"><i>Last edited: ' + edited + '</i></span>' +
        '</td></tr></table></a>';

      div.innerHTML = myContent;
    }
  };
  return {readyCallback, renderedCallback};
};

const {
  readyCallback: webResultsReadyCallback,
  renderedCallback: webResultsRenderedCallback,
} = makeTwoPartCallback();
window.__gcse || (window.__gcse = {});
window.__gcse = {
    parsetags: 'explicit',
    initializationCallback: myInitCallback,
    searchCallbacks: {
        web: {
            ready: webResultsReadyCallback,
            rendered: webResultsRenderedCallback,
        },
    },
};
</script>