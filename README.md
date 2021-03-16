// ==UserScript==
// @name         StarBreak: Sprite Sheet Replacer
// @namespace    http://ryara.net/
// @version      0.1
// @author       tobbez
// @match        https://www.starbreak.com/*
// @match        https://*.starbreak.com/*
// @grant        none
// ==/UserScript==


(function() {
    'use strict';


    /*
     * Add one entry below for each of the sprite sheets you want to replace (and replace/remove the example one).
     * The format of the name is name.index, for example forest.0 or lab.1.
     * The URL is the URL to the replacement sprite sheet.
     */
    let replacementTextures = {
              'base.0': 'https://i.imgur.com/pQ2ITLr.png',
        'base.1': 'https://i.imgur.com/DhQPAcI.png',
           'home.0': 'https://i.imgur.com/PFu8kiz.png',
        'home.1': 'https://i.imgur.com/vwI6K1n.png',

        'lobby.0': 'https://i.imgur.com/SLcHFXJ.png',
        'lobby.1': 'https://i.imgur.com/O7ZT5O4.png',

       //gy
        'hulk.0': 'https://i.imgur.com/4uZkDel.png',
        'hulk.1': 'https://i.imgur.com/nxNNlUc.png',
        'hulk.2': 'https://i.imgur.com/2ChbUoz.png',

       //st
        'jungle-s.0': 'https://i.imgur.com/17vppq3.png',
        'jungle.0': 'https://i.imgur.com/JUWUD12.png',
        'jungle.1': 'https://i.imgur.com/lEaDacA.png',
        'jungle.2': 'https://i.imgur.com/pgq7yvr.png',

       //ub
        'dumb-s.0': 'https://i.imgur.com/WkeKf8p.png',
        'dumb.0': 'https://i.imgur.com/sPHf8Bc.png',
        'dumb.1': 'https://i.imgur.com/WKRhUpp.png',
        'dumb.2': 'https://i.imgur.com/eCNY7bn.png',

       //gy arcade
        'wings.0': 'https://i.imgur.com/GIcn1sn.png',

       //sc
         //gold
          'scboss.0': 'https://i.imgur.com/bhq6xhh.png',
          'scboss.1': 'https://i.imgur.com/OoQe0sV.png',
          'scboss.2': 'https://i.imgur.com/EpZyr1u.png',
         //blue
          /*
          'scboss.0': 'https://i.imgur.com/1iyHUGp.png',
          'scboss.1': 'https://i.imgur.com/AY9GjPw.png',
          'scboss.2': 'https://i.imgur.com/9Kh7wJO.png',
          */

       //tl
        'lab-s.0': 'https://i.imgur.com/6GUD3Wm.png',
        'lab.0': 'https://i.imgur.com/6jnMZHu.png',
        'lab.1': 'https://i.imgur.com/0JTOnT0.png',
        'lab.2': 'https://i.imgur.com/J0GjlY0.png',

       //fc
        'cave.0': 'https://i.imgur.com/w0VDv3n.png',
        'cave.1': 'https://i.imgur.com/v1v3jvC.png',
        'cave.2': 'https://i.imgur.com/3X2Zw5s.png',

       //vq
        'city.1': 'https://i.imgur.com/EcwrmPY.png',
        'city.2': 'https://i.imgur.com/CEQcYrj.png',

       //fb
        'ice.0': 'https://i.imgur.com/F9tm2eM.png',
        'ice.1': 'https://i.imgur.com/tpgGzkn.png',
        'ice.2': 'https://i.imgur.com/itQPF3y.png',
        'ice.3': 'https://i.imgur.com/Oa8EAc2.png',

       //ff
        'forest.1': 'https://i.imgur.com/nXnMQ5R.png',

       //hope page
        'title.0': 'https://i.imgur.com/7YWwCpN.png',
    };


    function onLoaded () {
        XDL.loadTexture = function (filename, doneCallback, userData) {
            let textureName = filename.split('.').slice(0,2).join('.').split('/')[1];
            let realFilename = filename;
            if (textureName in replacementTextures) {
                realFilename = replacementTextures[textureName];
            }
            var image = new Image();


            image.crossOrigin = "Anonymous";


            image.src = realFilename;
            image.onload = function() {
                var textureId = XDL.renderEngine.addTexture(image);
                Runtime.dynCall('vii', doneCallback, [textureId, userData]);
            };
            image.onerror = function() {
                console.error('error trying to load ' + filename + ', retrying');
                setTimeout(function() {
                    XDL.loadTexture(filename, doneCallback, userData);
                }, 1000);
            };
        };
    }


    function waitUntilLoaded () {
        if (typeof XDL == 'undefined') {
            setTimeout(waitUntilLoaded, 100);
            return;
        }
        onLoaded();
    }


    waitUntilLoaded();
})();
