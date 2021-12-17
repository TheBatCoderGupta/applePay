/*
*
*  Push Notifications codelab
*  Copyright 2015 Google Inc. All rights reserved.
*
*  Licensed under the Apache License, Version 2.0 (the "License");
*  you may not use this file except in compliance with the License.
*  You may obtain a copy of the License at
*
*      https://www.apache.org/licenses/LICENSE-2.0
*
*  Unless required by applicable law or agreed to in writing, software
*  distributed under the License is distributed on an "AS IS" BASIS,
*  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
*  See the License for the specific language governing permissions and
*  limitations under the License
*
*/

/* eslint-env browser, serviceworker, es6 */

'use strict';

self.addEventListener('push', function (event) {
    console.log('[Service Worker] Push Received.');
    console.log(`[Service Worker] Push had this data: "${event.data.text()}"`);

    var pushMessage = `"${event.data.text()}"`;

    const title = 'eBusiness 6';
    //const options = {
    //    //   body: 'Yay it works.',
    //    body: pushMessage,
    //    icon: 'images/icon.png',
    //    badge: 'images/badge.png'
    //};

    const options = {
        "body": pushMessage,
        "icon": "images/icon.png",
        "badge": 'images/badge.png',
        "vibrate": [200, 100, 200, 100, 200, 100, 400],
        //"tag": "request",
        "tag": 'renotify',
        "renotify": true,
        "actions": [
            { "action": "yes", "title": "Yes", "icon": "images/yes.png" },
            { "action": "no", "title": "No", "icon": "images/no.png" }
        ]
    }

    //var notificationPromise = self.registration.showNotification(title, options);
    //notificationPromise.onclick = function (event) {
    //    event.preventDefault(); // prevent the browser from focusing the Notification's tab
    //    window.open('http://www.mozilla.org', '_blank');
    //}

    //event.waitUntil(notificationPromise);

    event.waitUntil(
        self.registration.showNotification(title, options)
    );
});

// Notification click event listener
self.addEventListener('notificationclick', e => {
    // Close the notification popout
    e.notification.close();
    // Get all the Window clients
    e.waitUntil(clients.matchAll({ type: 'window' }).then(clientsArr => {
        // If a Window tab matching the targeted URL already exists, focus that;
        //const hadWindowToFocus = clientsArr.some(windowClient => windowClient.url === e.notification.data.url ? (windowClient.focus(), true) : false);
        const hadWindowToFocus = clientsArr.some(windowClient => windowClient.url === e.notification.body ? (windowClient.focus(), true) : false);
        // Otherwise, open a new tab to the applicable URL and focus it.
        //if (!hadWindowToFocus) clients.openWindow(e.notification.data.url).then(windowClient => windowClient ? windowClient.focus() : null);
        var origUrl = e.notification.body.replace(/['"]+/g, '');
        var modifiedUrl = origUrl.substring(url.indexOf('ProductDetails.html?productId'));
		var appendQuery = "&isPushNotification=1";
        
		var finalurl = 'https://10.5.6.7/UI/' + modifiedUrl + appendQuery;
		
		if (!hadWindowToFocus) clients.openWindow(finalurl || "https://www.youtube.com").then(windowClient => windowClient ? windowClient.focus() : null);
    }));
});