# Catalyst Java SDK — Zia Services

> Source: https://docs.catalyst.zoho.com/en/sdk/java/v1/zia-services/
> Pages fetched: OCR (1), AutoML (1), Face-Analytics (1), Identity Scanner (5: Facial Comparison, Aadhaar, PAN, Passbook, Cheque), Text Analytics (4: Sentiment Analysis, NER, Keyword Extraction, All Text Analytics), Image-Moderation (1), Object-Recognition (1), Barcode Scanner (1)
> Total: 15 pages
> Date fetched: 2026-04-06

---

## Common Entry Point

All Zia Services use `ZCML.getInstance()` as the single entry point. Package: `com.zc.component.ml.ZCML`.

Note: Catalyst does not store any uploaded files. Files are used for one-time processing only and not for ML model training.

---

## 1. OCR (Optical Character Recognition)

Electronically detects textual characters in images/digital documents and converts to machine-encoded text. Supports 9 international + 10 Indian languages.

**Allowed formats**: .jpg, .jpeg, .png, .tiff, .bmp, .pdf  
**File size limit**: 20 MB

```java
import com.zc.component.ml.ZCContent;
import com.zc.component.ml.ZCLine;
import com.zc.component.ml.ZCML;
import com.zc.component.ml.ZCOCRModelType;
import com.zc.component.ml.ZCOCROptions;
import com.zc.component.ml.ZCParagraph;

File file = new File("MyImage.webp");
ZCOCROptions options = ZCOCROptions.getInstance()
    .setModelType(ZCOCRModelType.OCR)
    .setLanguageCode("eng,tam");
ZCContent ocrContent = ZCML.getInstance().getContent(file, options);
List<ZCParagraph> paragraphs = ocrContent.getParagraphs();
for (ZCParagraph p : paragraphs) {
    List<ZCLine> lines = p.lines;
    String text = p.text;
}
String rawText = ocrContent.text;
```

- `setModelType()` and `setLanguageCode()` are optional; defaults to OCR model, auto-detected language
- Response includes confidence score

---

## 2. AutoML

Train models and predict outcomes. Supports Binary Classification, Multi-Class Classification, and Regression.

**Note**: AutoML is NOT available in EU, AU, IN, JP, SA, or CA data centers.

```java
import com.zc.component.ml.ZCAutomlPredictionData;
import com.zc.component.ml.ZCML;

JSONObject obj = new JSONObject();
obj.put("column1", "value1");
obj.put("column2", "value2");
ZCAutomlPredictionData data = ZCML.getInstance().predictData(modelId, obj);
JSONObject classResult = data.getClassificationResultData(); // for classification
Object regResult = data.getRegressionResultData(); // for regression
```

---

## 3. Face Analytics

Facial detection and analysis: gender, age, emotion of detected faces.

**Allowed formats**: .jpg/.jpeg, .png  
**Modes**: BASIC, MODERATE, ADVANCED (default)

```java
import com.zc.component.ml.ZCFaceAnalysisData;
import com.zc.component.ml.ZCFaceAnalyticsOptions;
import com.zc.component.ml.ZCAnalyseMode;
import com.zc.component.ml.ZCML;

File file = new File("photo.jpg");
ZCFaceAnalyticsOptions options = ZCFaceAnalyticsOptions.getInstance()
    .setAgeNeeded(false)
    .setEmotionNeeded(true)
    .setGenderNeeded(true)
    .setAnalyseMode(ZCAnalyseMode.ADVANCED);
ZCFaceAnalysisData faceData = ZCML.getInstance().analyzeFace(file, options);
Long facesCount = faceData.getFacesCount();
List<ZCFaces> faces = faceData.getFacesList();
// Per face: getConfidence(), getAge(), getGender(), getEmotion(), getCoordinates(), getFaceLandmarks()
```

---

## 4. Identity Scanner

Secure identity checks on individuals and documents. Two categories: E-KYC (Facial Comparison) and Document Processing (Aadhaar, PAN, Passbook, Cheque).

### 4.1 Facial Comparison (E-KYC)

Compares two faces to determine if same individual. Available globally (console testing restricted to IN DC).

**Allowed formats**: .webp, .jpeg, .png  
**File size limit**: 10 MB

```java
import com.zc.component.ml.ZCFaceComparisonData;
import com.zc.component.ml.ZCML;

File source = new File("source.webp");
File query = new File("query.webp");
ZCFaceComparisonData data = ZCML.getInstance().compareFace(source, query);
Double confidence = data.getConfidence();
boolean matched = data.getMatched(); // true if confidence > 0.5
```

### 4.2 Aadhaar

Process Indian Aadhaar cards. **IN DC only** (not available in EU, AU, US, JP, SA, CA).

**Allowed formats**: .webp, .jpeg, .png, .bmp, .tiff, .pdf  
**File size limit**: 15 MB

```java
File front = new File("aadhaarFront.webp");
File back = new File("aadhaarBack.webp");
String languageCode = "eng,tam";
ZCContent ocrContent = ZCML.getInstance().getContentForAadhaar(front, back, languageCode);
// Response: name, address, gender, Aadhaar number with confidence scores
```

Note: Variables must be declared in order: aadhaarFront, aadhaarBack, languageCode. Language code parameter is being deprecated (auto-detection coming).

### 4.3 PAN

Process Indian PAN cards. **IN DC only**. English only.

**Allowed formats**: .webp, .jpeg, .png  
**File size limit**: 15 MB

```java
ZCOCROptions options = ZCOCROptions.getInstance().setModelType(ZCOCRModelType.PAN);
ZCContent ocrContent = ZCML.getInstance().getContent(file, options);
ZCPanData panData = ocrContent.getPanData();
String firstName = panData.getFirstName();
String lastName = panData.getLastName();
String pan = panData.getPan();
Date dob = panData.getDob();
```

### 4.4 Passbook

Process Indian bank passbooks. **IN DC only**. Supports 11 Indian + 8 international languages.

**Allowed formats**: .webp, .jpeg, .png, .bmp, .tiff, .pdf  
**File size limit**: 15 MB

```java
ZCOCROptions options = ZCOCROptions.getInstance()
    .setModelType(ZCOCRModelType.PASSBOOK)
    .setLanguageCode("tam");
ZCContent ocrContent = ZCML.getInstance().getContent(file, options);
// Response: bank name, branch, address, account number, RTGS/NEFT/IMPS flags
```

Note: Response always in English regardless of input language.

### 4.5 Cheque

Process Indian bank cheques (CTS-2010 format only). **IN DC only**. English only.

**Allowed formats**: .webp, .jpeg, .png  
**File size limit**: 15 MB

```java
ZCOCROptions options = ZCOCROptions.getInstance().setModelType(ZCOCRModelType.CHEQUE);
ZCContent ocrContent = ZCML.getInstance().getContent(file, options);
ZCChequeData chequeData = ocrContent.getChequeData();
String accountNumber = chequeData.getAccountNumber();
String ifsc = chequeData.getIfsc();
String bankName = chequeData.getBankName();
Long amount = chequeData.getAmount();
Date date = chequeData.getDate();
```

---

## 5. Text Analytics

NLP processing of text content. Three sub-features + combined endpoint.

### 5.1 Sentiment Analysis

Determines tone of text: positive, negative, or neutral. Sentence-level and overall analysis.

**Input limit**: 1500 characters per request.

```java
import com.zc.component.ml.ZCSentimentAnalysisData;
import com.zc.component.ml.ZCML;

JSONArray textArray = new JSONArray();
textArray.add("Great product experience...");
JSONArray keywords = new JSONArray(); // optional
keywords.add("product");
List<ZCSentimentAnalysisData> data = ZCML.getInstance().getSentimentAnalysis(textArray, keywords);
// Per result: getDocumentSentiment(), getOverallScore(), getSentenceAnalytics()
// Per sentence: getSentiment(), getSentence(), getConfidenceScore()
```

### 5.2 Named Entity Recognition (NER)

Extracts entities and categorizes them (organization, person, date, etc.).

**Input limit**: 1500 characters.

```java
import com.zc.component.ml.ZCNERData;
import com.zc.component.ml.ZCML;

JSONArray textArray = new JSONArray();
textArray.add("Zoho Corporation was founded by Sridhar Vembu...");
List<ZCNERData> nerData = ZCML.getInstance().getNERPrediction(textArray);
List<ZCNERDetails> details = nerData.get(0).getNERList();
// Per entity: getToken(), getNERTag(), getConfidenceScore(), getStartIndex(), getEndIndex()
```

### 5.3 Keyword Extraction

Extracts key words and phrases as text highlights/summary.

**Input limit**: 1500 characters.

```java
import com.zc.component.ml.ZCKeywordExtractionData;
import com.zc.component.ml.ZCML;

JSONArray textArray = new JSONArray();
textArray.add("Zoho Corporation is an Indian multinational...");
List<ZCKeywordExtractionData> data = ZCML.getInstance().getKeywordExtraction(textArray);
List keywords = data.get(0).getKeywords();
List keyphrases = data.get(0).getKeyphrases();
```

### 5.4 All Text Analytics (Combined)

Performs all three (Sentiment + NER + Keyword Extraction) in one call.

```java
import com.zc.component.ml.ZCTextAnalyticsData;
import com.zc.component.ml.ZCML;

JSONArray textArray = new JSONArray();
textArray.add("Zoho Corporation...");
JSONArray keywords = new JSONArray(); // optional for sentiment
keywords.add("Zoho");
List<ZCTextAnalyticsData> data = ZCML.getInstance().getTextAnalytics(textArray, keywords);
ZCTextAnalyticsData result = data.get(0);
result.getKeywordExtractionData();
result.getNERData();
result.getSentimentAnalysisData();
```

---

## 6. Image Moderation

Detects inappropriate/unsafe content: racy, nudity, violence, gore, weapons, drugs.

**Allowed formats**: .jpg/.jpeg, .png  
**Modes**: BASIC, MODERATE, ADVANCED (default)

```java
import com.zc.component.ml.ZCImageModerateData;
import com.zc.component.ml.ZCImageModerationOptions;
import com.zc.component.ml.ZCML;

File file = new File("image.jpg");
ZCImageModerationOptions options = ZCImageModerationOptions.getInstance()
    .setAnalyseMode(ZCAnalyseMode.ADVANCED);
ZCImageModerateData imData = ZCML.getInstance().moderateImage(file, options);
ZCImageModerationPrediction prediction = imData.getPrediction(); // safe_to_use or unsafe_to_use
Double confidence = imData.getConfidence();
List<ZCImageModerationConfidence> list = imData.getImageModerationConfidenceList();
```

---

## 7. Object Recognition

Detects, locates, and identifies objects in images. Can recognize 80 different object types.

**Allowed formats**: .jpg/.jpeg, .png

```java
import com.zc.component.ml.ZCObjectDetectionData;
import com.zc.component.ml.ZCML;

File file = new File("image.jpg");
List<ZCObjectDetectionData> objects = ZCML.getInstance().detectObjects(file);
for (ZCObjectDetectionData obj : objects) {
    String type = obj.getObjectType();
    Double confidence = obj.getConfidence();
    ZCObjectPoints coords = obj.getObjectPoints();
}
```

---

## 8. Barcode Scanner

Scans and decodes linear and 2D barcodes. Formats: Codabar, EAN-13, ITF, UPC-A, QR Code, and more.

**Allowed formats**: .jpg/.jpeg, .png

```java
import com.zc.component.ml.ZCBarcodeData;
import com.zc.component.ml.ZCBarcodeOptions;
import com.zc.component.ml.ZCBarcodeFormat;
import com.zc.component.ml.ZCML;

File file = new File("barcode.jpg");
ZCBarcodeOptions options = ZCBarcodeOptions.getInstance().setFormat(ZCBarcodeFormat.ALL);
ZCBarcodeData result = ZCML.getInstance().scanBarcode(file, options);
String content = result.getContent();
```
