# Java Library

* [How to retrieve Firebase Realtime data into a recyclerview](#Retrieving-Firebase-Data-in-Recyclerview)
* [How to start other app just with package name?](#How-to-start-other-app-just-with-package-name)
* [How to create SnackBar?](#Snackbar)
* [How to check internet connection?](#How-to-check-internet-connection)

## Retrieving-Firebase-data-in-Recyclerview
This is for showing Firebase Realtime data in an android recyclerview.

### 1. Connect your Firebase with your app 
In Android Studio go to top menu bar to "Tools > Firebase". In the right site bar click "Realtime Database" and "Connect" it with your online Firebase account.

### 2. Create user list example in Firebase

![here_should_be_the_example](https://github.com/emilianscheel/android-java-library/blob/main/firebase-user-list-example.png)

### 3. Copy this into your android application
```JAVA
ArrayList<UserItem> mList = new ArrayList<>();
DatabaseReference databaseReference = FirebaseDatabase.getInstance().getReference("user");
RecylerView recyclerView = findViewById(R.id.recyclerview);

recyclerView.setHasFixed(true);
recyclerView.setLayoutManager(new LinearLayoutManager(this));

databaseReference.addValueEventListener(new ValueEventListener() {
    @Override
    public void onDataChange(@NonNull DataSnapshot snapshot) {

        if (snapshot.hasChildren()) {

            for (DataSnapshot dataSnapshot : snapshot.getChildren()) {

                final UserItem user = dataSnapshot.getValue(UserItem.class);
                mList.add(user);
            }

        Adapter adapter = new Adapter(mList);
        recyclerView.setAdapter(adapter);
        adapter.notifyDataSetChanged();
        }
    }

    @Override
    public void onCancelled(@NonNull DatabaseError error) {

    }
});
```

### 4. Also copy the "Adapter Class", "Item Class" and the item resource file
Required for the Item Class is ...
* a empty "Constructor" and a full "Constructor"
* Getter and Setter for every String, Integer, Long, ...
```JAVA
public class UserItem {

    String user_name;
    String user_id;
    
    String joined_day;
    String joined_month;
    String joined_year;

    public UserItem(String user_name, String user_id, String joined_day, String joined_month, String joined_year) {
        this.user_name = user_name;
        this.user_id = user_id;
        this.joined_day = joined_day;
        this.joined_month = joined_month;
        this.joined_year = joined_year;
    }
    
    public String getUser_name() {
        return user_name;
    }

    public void setUser_name(String user_name) {
        this.user_name = user_name;
    }
    
    public String getUser_id() {
        return user_name;
    }

    public void setUser_id(String user_id) {
        this.user_id = user_id;
    }
    
    public String getJoined_day() {
        return joined_day;
    }

    public void setJoined_day(String joined_day) {
        this.joined_day = joined_day;
    }
    
    public String getJoined_month() {
        return joined_month;
    }

    public void setJoined_month(String joined_month) {
        this.joined_month = joined_month;
    }
    
    public String getJoined_year() {
        return joined_year;
    }

    public void setJoined_year(String joined_year) {
        this.joined_year = joined_year;
    }
    
}
```

```JAVA
public class UserAdapter extends RecyclerView.Adapter<FilterAdapter.ExampleViewHolder> {

    Context context;    
    ArrayList<UserItem> mList;
    
    public static class ExampleViewHolder extends RecyclerView.ViewHolder {
    
        public TextView tvUser_name;
        public TextView tvUser_id;

        public ExampleViewHolder(View itemView) {
            super(itemView);

            tvUser_name = itemView.findViewById(R.id.tvUserName);
            tvUser_id = itemView.findViewById(R.id.tvUserId);
         }
    }

    public FilterAdapter(Context context, ArrayList<UserItem> mList) {
        this.context = context;
        this.mList = mList;
    }

    @Override
    public ExampleViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        View v = LayoutInflater.from(parent.getContext()).inflate(R.layout.item_user, parent, false);
        ExampleViewHolder evh = new ExampleViewHolder(v);
        return evh;
    }

    @Override
    public void onBindViewHolder(final ExampleViewHolder holder, final int position) {

        final UserItem user = mList.get(position);

        holder.tvUser_name.setText(user.getUser_name());
        holder.tvUser_id.setText(user.getUser_id());
    }

    @Override
    public int getItemCount() {
        return mList.size();
    }
}

```

```XML
<?xml version="1.0" encoding="utf-8"?>

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:orientation="horizontal">

        <TextView
            android:id="@+id/tvUserName"
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:text="Jolix"
            android:textColor="#000"
            android:textSize="14dp" />
            
        <TextView
            android:id="@+id/tvUserId"
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:text="032489203"
            android:textColor="#000"
            android:textSize="14dp" />

</LinearLayout>
```
## Snackbar
This is for showing custom user messages.

### 4. Java Example

```JAVA
int seconds = 2;

Snackbar snackbar = Snackbar.make(view, "Hello World!", 1000 * seconds);
View sbView = snackbar.getView();
TextView textView = (TextView) sbView.findViewById(R.id.snackbar_text);

// set font-family

textView.setTypeface(ResourcesCompat.getFont(this, R.font.m));

// set text-color

textView.setTextColor(ContextCompat.getColor(this, R.color.white));

// set icon drawable

textView.setCompoundDrawablesWithIntrinsicBounds(R.drawable.icon, 0, 0, 0);            textView.setCompoundDrawablePadding(getResources().getDimensionPixelOffset(R.dimen.snack_bar_icon_padding));

// set Snackbar background

sbView.setBackgroundColor(ContextCompat.getColor(this, R.color.colorPrimary));

// show Snackbar

snackbar.show();
```

## How to start other app just with package name?
```JAVA
String package_name = "com.package.name";

Intent intent = getPackageManager().getLaunchIntentForPackage(package_name);

    if (intent != null) {
        
        // Packe exits  --->  open this app

        intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);

    } else {
        
        // Package doesnt exits  --->  open this package in Play Store
            
        intent = new Intent(Intent.ACTION_VIEW);
        intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
        intent.setData(Uri.parse("https://play.google.com/store/apps/details?id=" + package_name));
    }
    
startActivity(intent);
```
## How to check internet connection?

Create this ```isWifiConnected(this)``` method.
```JAVA
public Boolean isWifiConnected(Context context) {
        
    try {
            
         // Todo ---> check if internet isConnected
            
         ConnectivityManager cm = (ConnectivityManager)context.getSystemService(Context.CONNECTIVITY_SERVICE);
         NetworkInfo nInfo = cm.getActiveNetworkInfo();
         Boolean isConnected = nInfo != null && nInfo.isAvailable() && nInfo.isConnected();
         return isConnected;
            
    } catch (Exception e) {
            
         // Todo ---> There is no internet connectivity
           
         Log.e("Connectivity Exception", e.getMessage());

         return false;
    }
}
```
How to use it?
```JAVA
if (isWifiConnected()) {

    // do something
    
} else {

   // do something
}

