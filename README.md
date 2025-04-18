********************************************************* Mobile App Dev*************************************************

slip15a

activity_main.xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:padding="20dp"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <EditText
        android:id="@+id/etNum1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter Number 1"
        android:inputType="numberDecimal"/>

    <EditText
        android:id="@+id/etNum2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter Number 2"
        android:inputType="numberDecimal"
        android:layout_marginBottom="20dp"/>

    <Button
        android:id="@+id/btnAdd"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="ADD" />

    <Button
        android:id="@+id/btnSub"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="SUB" />

    <Button
        android:id="@+id/btnMul"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="MULT" />

    <Button
        android:id="@+id/btnDiv"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="DIV" />
</LinearLayout>



MainActivity.java

package com.example.calculatorapp;

import android.os.Bundle;
import android.widget.*;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

EditText etNum1, etNum2;
Button btnAdd, btnSub, btnMul, btnDiv;

@Override
protected void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);
setContentView(R.layout.activity_main);

etNum1 = findViewById(R.id.etNum1);
etNum2 = findViewById(R.id.etNum2);
btnAdd = findViewById(R.id.btnAdd);
btnSub = findViewById(R.id.btnSub);
btnMul = findViewById(R.id.btnMul);
btnDiv = findViewById(R.id.btnDiv);

btnAdd.setOnClickListener(v -> performOperation("+"));
btnSub.setOnClickListener(v -> performOperation("-"));
btnMul.setOnClickListener(v -> performOperation("*"));
btnDiv.setOnClickListener(v -> performOperation("/"));
}

private void performOperation(String op) {
String num1Str = etNum1.getText().toString();
String num2Str = etNum2.getText().toString();

if (num1Str.isEmpty() || num2Str.isEmpty()) {
Toast.makeText(this, "Please enter both numbers", Toast.LENGTH_SHORT).show();
return;
}

double num1 = Double.parseDouble(num1Str);
double num2 = Double.parseDouble(num2Str);
double result = 0;

switch (op) {
case "+": result = num1 + num2; break;
case "-": result = num1 - num2; break;
case "*": result = num1 * num2; break;
case "/":
if (num2 == 0) {
Toast.makeText(this, "Cannot divide by zero", Toast.LENGTH_SHORT).show();
return;
}
result = num1 / num2;
break;
}

Toast.makeText(this, "Result: " + result, Toast.LENGTH_SHORT).show();
}
}


-------------------------------------------------------------------------------------------------------
slip15b

activity_main.xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:padding="20dp"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/tvHeading"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Bank App"
        android:textSize="24sp"
        android:textStyle="bold"
        android:layout_gravity="center"
        android:layout_marginBottom="20dp"/>

    <EditText
        android:id="@+id/etAmount"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter amount"
        android:inputType="numberDecimal" />

    <Button
        android:id="@+id/btnDeposit"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Deposit"
        android:layout_marginTop="15dp"/>

    <Button
        android:id="@+id/btnWithdraw"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Withdraw" />

    <Button
        android:id="@+id/btnCheckBalance"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Check Balance" />
</LinearLayout>


MainActivity.java

package com.example.bankapp;

import android.os.Bundle;
import android.widget.*;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
EditText etAmount;
Button btnDeposit, btnWithdraw, btnCheckBalance;
double balance = 0.0;  

@Override
protected void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);
setContentView(R.layout.activity_main);

        
etAmount = findViewById(R.id.etAmount);
btnDeposit = findViewById(R.id.btnDeposit);
btnWithdraw = findViewById(R.id.btnWithdraw);
btnCheckBalance = findViewById(R.id.btnCheckBalance);

btnDeposit.setOnClickListener(v -> {
String amtStr = etAmount.getText().toString();
if (amtStr.isEmpty()) {
Toast.makeText(this, "Enter amount to deposit", Toast.LENGTH_SHORT).show();
} 
else 
{
double amount = Double.parseDouble(amtStr);
balance += amount;
Toast.makeText(this, "Deposited ₹" + amount, Toast.LENGTH_SHORT).show();
etAmount.setText(""); 
}
});

btnWithdraw.setOnClickListener(v -> {
String amtStr = etAmount.getText().toString();
if (amtStr.isEmpty()) {
Toast.makeText(this, "Enter amount to withdraw", Toast.LENGTH_SHORT).show();
} 
else 
{
double amount = Double.parseDouble(amtStr);
if (amount > balance) {
Toast.makeText(this, "Insufficient balance", Toast.LENGTH_SHORT).show();
} 
else 
{
balance -= amount;
Toast.makeText(this, "Withdrew ₹" + amount, Toast.LENGTH_SHORT).show();
etAmount.setText(""); // clear the field
}
}
});

btnCheckBalance.setOnClickListener(v -> {
Toast.makeText(this, "Current Balance: ₹" + balance, Toast.LENGTH_SHORT).show();
 });
}
}


***********************************************************************************************************************

slip22a  (app > src > main > res > drawable)

activity_main.xml

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:padding="16dp">

    <!-- ImageView to display the image -->
    <ImageView
        android:id="@+id/imageView"
        android:layout_width="200dp"
        android:layout_height="200dp"
        android:src="@drawable/image1" />

    <!-- Button to change the image -->
    <Button
        android:id="@+id/buttonChangeImage"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Change Image"
        android:layout_marginTop="20dp"/>

</LinearLayout>


MainActivity.java

package com.example.changeimageapp;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.ImageView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

private ImageView imageView;
private Button buttonChangeImage;

@Override
protected void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);
setContentView(R.layout.activity_main);

      
imageView = findViewById(R.id.imageView);
buttonChangeImage = findViewById(R.id.buttonChangeImage);

buttonChangeImage.setOnClickListener(new View.OnClickListener() {

@Override
public void onClick(View v) {
imageView.setImageResource(R.drawable.image2);
}
});
}
}

------------------------------------------------------------------------------------------------------------
slip22b

activity_main.xml

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:padding="20dp"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center">

    <!-- EditText for the first number -->
    <EditText
        android:id="@+id/etNum1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter Number 1"
        android:inputType="numberDecimal"
        android:layout_marginBottom="10dp" />

    <!-- EditText for the second number -->
    <EditText
        android:id="@+id/etNum2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter Number 2"
        android:inputType="numberDecimal"
        android:layout_marginBottom="20dp" />

    <!-- RadioGroup for operations -->
    <RadioGroup
        android:id="@+id/rgOperations"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:layout_marginBottom="20dp">

        <!-- RadioButton for addition -->
        <RadioButton
            android:id="@+id/rbAdd"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Add"
            android:checked="true"/>

        <!-- RadioButton for subtraction -->
        <RadioButton
            android:id="@+id/rbSub"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Subtract" />

        <!-- RadioButton for multiplication -->
        <RadioButton
            android:id="@+id/rbMul"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Multiply" />

        <!-- RadioButton for division -->
        <RadioButton
            android:id="@+id/rbDiv"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Divide" />
    </RadioGroup>

    <!-- Button to perform the operation -->
    <Button
        android:id="@+id/btnCalculate"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Calculate" />

    <!-- TextView to display the result -->
<TextView
        android:id="@+id/tvResult"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Result: "
        android:layout_marginTop="20dp"
        android:textSize="18sp"/>
</LinearLayout>


MainActivity.java (Java Code)

package com.example.numericoperations;

import android.os.Bundle;
import android.view.View;
import android.widget.*;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

EditText etNum1, etNum2;
RadioGroup rgOperations;
RadioButton rbAdd, rbSub, rbMul, rbDiv;
Button btnCalculate;
TextView tvResult;

@Override
protected void onCreate(Bundle savedInstanceState) {
super.onCreate(savedInstanceState);
setContentView(R.layout.activity_main);

    
etNum1 = findViewById(R.id.etNum1);
etNum2 = findViewById(R.id.etNum2);
rgOperations = findViewById(R.id.rgOperations);
rbAdd = findViewById(R.id.rbAdd);
rbSub = findViewById(R.id.rbSub);
rbMul = findViewById(R.id.rbMul);
rbDiv = findViewById(R.id.rbDiv);
btnCalculate = findViewById(R.id.btnCalculate);
tvResult = findViewById(R.id.tvResult);

       
btnCalculate.setOnClickListener(v -> {
String num1Str = etNum1.getText().toString();
String num2Str = etNum2.getText().toString();

if (num1Str.isEmpty() || num2Str.isEmpty()) {
Toast.makeText(MainActivity.this, "Please enter both numbers", Toast.LENGTH_SHORT).show();
return;
}

double num1 = Double.parseDouble(num1Str);
double num2 = Double.parseDouble(num2Str);
double result = 0;

int selectedOperation = rgOperations.getCheckedRadioButtonId();
switch (selectedOperation) {
case R.id.rbAdd:
result = num1 + num2;
break;
case R.id.rbSub:
result = num1 - num2;
break;
case R.id.rbMul:
result = num1 * num2;
break;
case R.id.rbDiv:
if (num2 == 0) {
Toast.makeText(MainActivity.this, "Cannot divide by zero", Toast.LENGTH_SHORT).show();
return;
}
result = num1 / num2;
break;
}

          
tvResult.setText("Result: " + result);
});
}
}

***********************************************************************************************************************


