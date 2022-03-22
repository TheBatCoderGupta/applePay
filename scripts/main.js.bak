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

/* eslint-env browser, es6 */

'use strict';

const applicationServerPublicKey = 'BBlVq-YXLap4RrJcGKXRX7001DHvcFxRJl6GAdqFe_wC4wDy85WzpS6OBqyS07yfBJfPAWRFPnS0xdVSERxZCPU';//'BAoUiWCHLG9YDW5q_ZEAYQPjd7r2bLWurJbNkYLeg8H95c2tfY_TfVwWSMO7H_msAvCIwUr9Xgri8q2SIGm5N04';

let isSubscribed = false;
let swRegistration = null;

function urlB64ToUint8Array(base64String) {
  const padding = '='.repeat((4 - base64String.length % 4) % 4);
  const base64 = (base64String + padding)
    .replace(/\-/g, '+')
    .replace(/_/g, '/');

  const rawData = window.atob(base64);
  const outputArray = new Uint8Array(rawData.length);

  for (let i = 0; i < rawData.length; ++i) {
    outputArray[i] = rawData.charCodeAt(i);
  }
  return outputArray;
}

if ('serviceWorker' in navigator && 'PushManager' in window) {
  console.log('Service Worker and Push are supported');

  navigator.serviceWorker.register('sw.js')
  .then(function(swReg) {
    console.log('Service Worker is registered', swReg);

    swRegistration = swReg;
  })
  .catch(function(error) {
    console.error('Service Worker Error', error);
  });
} else {
  console.warn('Push messaging is not supported');
}

function initializeUI() {
    window.addEventListener('load', function () {
        // Set the initial subscription value
        swRegistration.pushManager.getSubscription()
            .then(function (subscription) {
                isSubscribed = !(subscription === null);

                if (isSubscribed) {
                    console.log('User IS subscribed.');
                } else {
                    console.log('User is NOT subscribed.');
                    subscribeUser();
                }
            });
    });
}

function subscribeUser() {
  const applicationServerKey = urlB64ToUint8Array(applicationServerPublicKey);
  swRegistration.pushManager.subscribe({
    userVisibleOnly: true,
    applicationServerKey: applicationServerKey
  })
  .then(function(subscription) {
    console.log('User is subscribed.');

    updateSubscriptionOnServer(subscription);

    isSubscribed = true;

  })
  .catch(function(error) {
    console.error('Failed to subscribe the user: ', error);
  });
}

function updateSubscriptionOnServer(subscription) {
  // TODO: Send subscription to application server
    if (subscription) {
        const subscriptionJSON = subscription.toJSON();
        var dataToSend = {
            "PushEndPoint": subscriptionJSON.endpoint,
            "p256dh": subscriptionJSON.keys.p256dh,
            "Auth": subscriptionJSON.keys.auth
        };

        $.ajax({
            url: 'https://10.5.6.7/SOA/v1/addPushSubscription',
            type: "POST",
            data: dataToSend,
            success: function () {
                console.log("success");
            },
            error: function () {
                console.log("failure");
            }
        });
    }
    console.log("Subscription: " + JSON.stringify(subscription));  
}

navigator.serviceWorker.register('sw.js')
.then(function(swReg) {
  console.log('Service Worker is registered', swReg);

  swRegistration = swReg;
  initializeUI();
})
