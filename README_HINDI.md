# IND Android App — शुरुआती प्रोजेक्ट

यह Android Studio के लिए Kotlin + Jetpack Compose में बनाया गया शुरुआती UI प्रोजेक्ट है।

## अभी क्या काम करता है
- Home feed का डेमो
- Stories की पट्टी
- लाइक बटन (लोकल डेमो state)
- Explore स्क्रीन और सर्च टेक्स्ट बॉक्स
- Create पोस्ट का कैप्शन ड्राफ्ट
- Reels और Profile स्क्रीन का UI
- Creator Studio / कमाई डैशबोर्ड का शुरुआती UI
- Settings में स्थानीय हिंदी/English चयन और Dark/Light Mode टॉगल
- प्रस्तावित Creator नियम: 1,000 फॉलोअर्स, 30 दिनों में 10,000 वैध व्यू, मासिक भुगतान और ₹500 न्यूनतम सीमा
- प्रस्तावित IND Verified सदस्यता: ₹49/महीना (पहचान सत्यापन फिर भी आवश्यक)
- IND लोगो कॉन्सेप्ट `app/src/main/assets/ind-logo-concept.png` में
- पाँच टैब वाला नेविगेशन

## अभी क्या डेमो/अधूरा है
Creator Studio में ₹0.00 केवल डेमो बैलेंस है; कोई वास्तविक कमाई या भुगतान नहीं होता। 1,000 फॉलोअर्स, 30 दिनों में 10,000 वैध व्यू, मासिक भुगतान और ₹500 न्यूनतम सीमा केवल प्रस्तावित शुरुआती नियम हैं—इन्हें सर्वर पर लागू नहीं किया गया है। ₹49/महीना Verified सदस्यता भी केवल योजना है; भुगतान या पहचान सत्यापन चालू नहीं है। विज्ञापन, प्रीमियम, लाइव गिफ्ट और ब्रांड पार्टनरशिप अभी प्रस्तावित कमाई के तरीके हैं।

यह अभी लाइव सोशल नेटवर्क नहीं है। भाषा/थीम का चुनाव केवल इस डेमो रन में स्थानीय UI state है; सभी स्क्रीन का पूर्ण अनुवाद और preference persistence बाकी है। अकाउंट (Google/मोबाइल OTP/ईमेल), असली फोटो/वीडियो अपलोड, सर्वर-साइड लाइक/कमेंट, चैट, वीडियो कॉल, लाइव स्ट्रीम, विज्ञापन, सब्सक्रिप्शन, बैंक पेमेंट, नोटिफिकेशन, रिपोर्ट/ब्लॉक और मॉडरेशन को Firebase/backend तथा अन्य सेवाओं से जोड़ना बाकी है। डेमो में दिखने वाले यूज़र और पोस्ट नकली नमूने हैं।

## Android Studio में चलाएँ
1. Android Studio में इस फ़ोल्डर को Open करें।
2. Gradle sync पूरा होने दें।
3. Android SDK Platform 35 इंस्टॉल रखें।
4. Emulator या Android फोन चुनकर Run दबाएँ।

## लाइव बैकएंड जोड़ने के चरण
1. Firebase Console में नया project `IND` बनाएँ।
2. Android app जोड़ें; package name `com.ind.social` रखें।
3. Firebase Authentication में Email/Password और Google provider चालू करें।
4. Cloud Firestore और Firebase Storage चालू करें।
5. Firebase Android BoM, Auth, Firestore और Storage dependencies जोड़ें।
6. Firebase Console से `google-services.json` डाउनलोड करके `app/` में रखें और Google Services Gradle plugin configure करें।
7. नीचे दी गई Firestore और Storage rules को समीक्षा के बाद deploy करें।

## सुझाया गया Firestore डेटा मॉडल
- `users/{uid}`: username, displayName, bio, photoURL, createdAt
- `posts/{postId}`: uid, caption, mediaURL, mediaType, createdAt, likeCount
- `posts/{postId}/comments/{commentId}`: uid, text, createdAt
- `posts/{postId}/likes/{uid}`: createdAt
- `users/{uid}/following/{targetUid}`: createdAt
- `chats/{chatId}/messages/{messageId}`: senderId, text, mediaURL, createdAt
- `reports/{reportId}`: reporterUid, targetType, targetId, reason, createdAt

## लॉन्च से पहले जरूरी सुरक्षा
- username uniqueness को trusted backend/Cloud Function से लागू करें।
- चैट और निजी डेटा पर participant-only access रखें।
- uploads का आकार, MIME type और ownership validate करें।
- rate limiting, spam controls, reporting, blocking और moderation जोड़ें।
- बच्चों/नाबालिगों, privacy, copyright और स्थानीय कानूनों के लिए नीति/प्रक्रिया बनाएं।
- Firestore/Storage rules को emulator में test करें; production में खुली read/write rules कभी deploy न करें।
- App signing key सुरक्षित रखें; इसे सार्वजनिक रूप से साझा न करें।

## नया IND लोगो

नया मॉडर्न एब्स्ट्रैक्ट लोगो ऐप के launcher icon (सभी सामान्य Android densities) और `app/src/main/assets/ind-logo-concept.png` में लगाया गया है। यह source project update है; APK अभी build नहीं किया गया है।


नया IND लोगो: अपडेटेड लोगो में “Connect • Share • Grow” टैगलाइन हटा दी गई है।

## मोबाइल से APK बनाने का तरीका (GitHub Actions)
1. इस प्रोजेक्ट ZIP को फोन में डाउनलोड करके Extract करें।
2. GitHub पर नया Public या Private repository बनाएं।
3. Extract किए हुए प्रोजेक्ट की सभी फाइलें/फ़ोल्डर repository में अपलोड करें (सिर्फ ZIP अपलोड न करें)।
4. GitHub repository में Actions टैब खोलें। `Build IND APK` workflow चुनें और `Run workflow` दबाएँ।
5. Build पूरा होने पर workflow run के नीचे `IND-debug-APK` artifact डाउनलोड करें। ZIP को Extract करें; अंदर `app-debug.apk` मिलेगा।
6. APK खोलकर इंस्टॉल करें। फोन पूछे तो ब्राउज़र/फ़ाइल मैनेजर को इस स्रोत से ऐप इंस्टॉल करने की अनुमति दें।

यह Debug APK होगा। Firebase और वास्तविक ऑनलाइन सुविधाएँ अलग से जोड़नी होंगी।
