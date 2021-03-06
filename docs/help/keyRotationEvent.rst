#################
keyRotationEvent
#################
This function performs a key rotation event. In a rotation event the previous pre-rotated key becomes the new current
key and a new pre-rotated key is created (either manually or automatically generated). After a rotation event the old
current key can no longer be used to sign data. This function also contains a number of options for managing the key
pairs. One can post the new key data to one or more didery servers, as well as show or save the private keys.

Parameters
==========
keyRotationEvent has three required parameters. The first is a 64 byte Uint8Array of the current private key, the
second is a 64 byte Uint8Array of the pre-rotated key, and the third is the DID string associated with the given keys.
This function also has one optional parameter which is an object containing various options. The possible options are as
follows:

+------------------+---------------------------------------------------------------------------------------------------+
|      Option      |                                            Description                                            |
+==================+===================================================================================================+
|seed              |A 32 byte Uint8Array or string used as seed for the new pre-rotated key pair generation.           |
|                  |key pair will be used as the current key pair.                                                     |
+------------------+---------------------------------------------------------------------------------------------------+
|preRotatedKeyPair |An array with a 64 byte Uint8Array of a private key and 32 byte Uint8Array of a public key. This   |
|                  |key pair will be used as the new pre-rotated key pair.                                             |
+------------------+---------------------------------------------------------------------------------------------------+
|post              |A boolean for whether or not key data should be posted to a didery server.                         |
+------------------+---------------------------------------------------------------------------------------------------+
|consensus         |A float between 0 and 1 for the consensus threshold used when retrieving key history (key history  |
|                  |is only retrieved when posting data).                                                              |
+------------------+---------------------------------------------------------------------------------------------------+
|urls              |An array of comma separated server URLs strings.                                                   |
+------------------+---------------------------------------------------------------------------------------------------+
|saveCurrent       |A boolean for whether or not the new current private key should be saved somewhere. If true a      |
|                  |storage location must be provided.                                                                 |
+------------------+---------------------------------------------------------------------------------------------------+
|savePreRotated    |A boolean for whether or not the bew pre-rotated private key should be saved somewhere. If true a  |
|                  |storage location must be provided.                                                                 |
+------------------+---------------------------------------------------------------------------------------------------+
|storageCurrent    |The current private key storage location; Accepted values include "local", "session" or "download".|
+------------------+---------------------------------------------------------------------------------------------------+
|storagePreRotated |The pre-rotated private key storage location; Accepted values include "local", "session" or        |
|                  |"download".                                                                                        |
+------------------+---------------------------------------------------------------------------------------------------+
|showCurrent       |A boolean for whether or not to show the new current private key in an alert.                      |
+------------------+---------------------------------------------------------------------------------------------------+
|showPreRotated    |A boolean for whether or not to show the new pre-rotated private key in an alert.                  |
+------------------+---------------------------------------------------------------------------------------------------+

Return
======
keyRotationEvent returns a promise that when fulfilled returns an array with new pre-rotated key pair:
``[Uint8Array[pre-rotated private], Uint8Array[pre-rotated public]]``.

Example
=======
.. code-block:: javascript

   const didery = require('didery');

   let oldk = new Uint8Array([70,81,79,121,66,71,103,69,120,52,89,112,52,102,78,51,54,68,117,70,109,106,87,49,107,55,113,
                             75,79,86,111,101,167,185,202,28,236,26,127,61,230,20,129,200,113,50,88,24,161,11,216,134,
                             159,167,151,183,94,25,189,11,128,151,39,237]);
   let newk = new Uint8Array([188,89,18,248,82,201,46,115,209,235,210,41,149,50,159,180,160,116,132,133,125,134,226,208,
                             176,15,83,159,113,216,145,30,157,63,39,51,235,12,209,233,92,5,64,118,42,141,40,58,154,52,
                             155,184,49,132,74,177,123,242,187,69,247,206,115,8]);
   let did = "did:dad:wqfCucOKHMOsGn89w6YUwoHDiHEyWBjCoQvDmMKGwp="
   let options = {};
   options.seed = "FQOyBGgEx4Yp4fN36DuFmjW1k7qKOVoe";
   didery.keyRotationEvent(oldk, newk, did, options).then(function (response) {
        console.log(response);
   });
   // [Uint8Array[70,81,79,121,66,71,103,69,120,52,89,112,52,102,78,51,54,68,117,70,109,
   // 106,87,49,107,55,113,75,79,86,111, 101,167,185,202,28,236,26,127,61,230,20,129,200,113,50,88,24,161,11,216,134,
   // 159,167,151,183,94,25,189,11,128,151,39,237], Uint8Array[167,185,202,28,236,26,127,61,230,20,129,200,113,50,88,24,
   // 161,11,216,134,159,167,151,183,94,25,189,11,128,151,39,237]]

   oldk = new Uint8Array([70,81,79,121,66,71,103,69,120,52,89,112,52,102,78,51,54,68,117,70,109,106,87,49,107,55,113,75,
                         79,86,111,101,167,185,202,28,236,26,127,61,230,20,129,200,113,50,88,24,161,11,216,134,159,167,
                         151,183,94,25,189,11,128,151,39,237]);
   newk Uint8Array([188,89,18,248,82,201,46,115,209,235,210,41,149,50,159,180,160,116,132,133,125,134,226,208,176,15,83,
                   159,113,216,145,30,157,63,39,51,235,12,209,233,92,5,64,118,42,141,40,58,154,52,155,184,49,132,74,177,
                   123,242,187,69,247,206,115,8]);
   did = "did:dad:wqfCucOKHMOsGn89w6YUwoHDiHEyWBjCoQvDmMKGwp="
   options = {};
   options.seed = "FQOyBGgEx4Yp4fN36DuFmjW1k7qKOVoe";
   options.post = true;
   options.consensus = 0.75;
   options.urls = ["http://127.0.0.1:8080/"];
   options.saveCurrent = true;
   options.savePreRotated = true;
   options.storageCurrent = "local";
   options.storagePreRotated = "local";
   options.showCurrent = false;
   options.showPreRotated = false;
   didery.keyRotationEvent(oldk, newk, did, options).then(function (response) {
        console.log(response);
   });
   // [Uint8Array[70,81,79,121,66,71,103,69,120,52,89,112,52,102,78,51,54,68,117,70,109,
   // 106,87,49,107,55,113,75,79,86,111, 101,167,185,202,28,236,26,127,61,230,20,129,200,113,50,88,24,161,11,216,134,
   // 159,167,151,183,94,25,189,11,128,151,39,237], Uint8Array[167,185,202,28,236,26,127,61,230,20,129,200,113,50,88,24,
   // 161,11,216,134,159,167,151,183,94,25,189,11,128,151,39,237]]
