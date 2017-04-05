# [Android] Video play
This project provide for android developer to know the easier way to have video player sample


## Implementation

### Part 1

Ref: http://stackoverflow.com/questions/6568054/how-to-write-a-simple-video-player-on-android

1. Create video view

	```
    <VideoView android:id="@+id/videoView"
    android:layout_height="match_parent"
    android:layout_width="match_parent"/>
	```

1. Source code in MainActivity. Init MediaController to play video

    ```
    MediaController mediaController = new MediaController(this);
    mediaController.setAnchorView(videoView);
	
    Uri uri = Uri.fromFile(new File(filename));
    Log.d(TAG, "playVideo: uri=" + uri);
	
    videoView.setMediaController(mediaController);
    videoView.setVideoURI(uri);
    videoView.requestFocus();
	
    videoView.start();
	```

### Part2: for external permission (C&P)

1. Under AndroidManifest.xml add

	```
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
	```    

2. To check external permission use below source code.  
	Specify your required permission in String[] array.  

	```
    public  boolean isStoragePermissionGranted() {
        if (Build.VERSION.SDK_INT >= 23) {
            if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE)
                    == PackageManager.PERMISSION_GRANTED) {
                Log.v(TAG,"Permission is granted");
                return true;
            } else {
	
                Log.v(TAG,"Permission is revoked");
                ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.WRITE_EXTERNAL_STORAGE}, 1);
                return false;
            }
        }
        else { //permission is automatically granted on sdk<23 upon installation
            Log.v(TAG,"Permission is granted");
            return true;
        }
    }
	```

3. After get the response

	```
    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        if(grantResults[0]== PackageManager.PERMISSION_GRANTED){
            Log.v(TAG,"Permission: "+permissions[0]+ "was "+grantResults[0]);
            //resume tasks needing this permission
            playVideo(videoView);
        }
    }
	```


# Postscript
1. 2017/04/05 add the 1st version