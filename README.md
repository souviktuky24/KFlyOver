# react-native-flyover

A simple React Native native flyover for invoking `Activity.startActivityForResult`, `Activity.setResult`, and `Activity.finish` to help with implementing app-to-app communication.

## Getting started

`$ npm install react-native-flyover --save`

### Mostly automatic installation

`$ react-native link react-native-flyover`

### Manual installation

#### Android

1. Open up `android/app/src/main/java/[...]/MainActivity.java`
  - Add `import com.kashish.FlyOverPackage;` to the imports at the top of the file
  - Add `new FlyOverPackage()` to the list returned by the `getPackages()` method
2. Append the following lines to `android/settings.gradle`:
  	```
  	include ':react-native-flyover'
  	project(':react-native-flyover').projectDir = new File(rootProject.projectDir, 	'../node_modules/react-native-flyover/android')
  	```
3. Insert the following lines inside the dependencies block in `android/app/build.gradle`:
  	```
      compile project(':react-native-flyover')
  	```

## Usage
```javascript
import FlyOver from 'react-native-flyover';

// Check if an app is installed to handle an intent
const flyover = await FlyOver.resolveFlyOver('com.kashish.TARGET_INTENT');
if (!flyover) {
	console.warn('Please install the othe app.');
} else {
	console.log(`Activity will be handled by ${activity.package}`);
}

// naked startActivity replacement

FlyOver.startFlyOver('com.kashish.TARGET_INTENT', args);

// Start an activity for a result
let uniqueId = 0;
let args = /* ... */;
const response = await FlyOver.startFlyOverForResult(++uniqueId, 'com.kashish.TARGET_INTENT', args);
if (response.resultCode !== FlyOver.OK) {
  throw new Error('Invalid result from activity.');
} else {
  console.log('Got the following response: ' + response.data);
}

// Finish an activity with a result
KFlyOver.destroyFlyOver(FlyOver.OK, 'com.kashish.TARGET_INTENT', args);
```
