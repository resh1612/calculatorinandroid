# Ex_5_-Develop a simple calculator using android studio.
Develop a program to create a simple calculator using Android Studio.

## AIM:
To develop a program to create a simple calculator using Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min. required Artic Fox)


## ALGORITHM:
Step 1: Open Android Studio and then click on File -> New -> New project.

Step 2: Then type the Application name as SMSIntent and click Next.

Step 3: Select the Minimum SDK below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally, click Finish.

Step 5: Design layout in activity_main.xml.

Step 6: Perform the Calculator Operation in MainActivity.java

Step 7: Save and run the application.

## PROGRAM:
MainActivity.java
```
package com.example.calculator;

import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import java.util.Stack;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    TextView display;
    String input = "";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        display = findViewById(R.id.display);

        int[] buttonIds = {
                R.id.btn0,R.id.btn1,R.id.btn2,R.id.btn3,R.id.btn4,R.id.btn5,R.id.btn6,R.id.btn7,R.id.btn8,R.id.btn9,
                R.id.btnAdd,R.id.btnSub,R.id.btnMul,R.id.btnDiv,R.id.btnDot,R.id.btnEqual,R.id.btnC,R.id.btnBack,R.id.btnPercent
        };

        for (int id : buttonIds) {
            findViewById(id).setOnClickListener(this);
        }
    }

    @Override
    public void onClick(View v) {
        Button b = (Button) v;
        String buttonText = b.getText().toString();

        switch (buttonText) {
            case "C":
                input = "";
                break;
            case "⌫":
                if (!input.isEmpty())
                    input = input.substring(0, input.length() - 1);
                break;
            case "=":
                try {
                    double result = evaluate(input);
                    if(result == (long)result)
                        input = String.valueOf((long)result);
                    else
                        input = String.valueOf(result);
                } catch (Exception e) {
                    input = "Error";
                }
                break;
            default:
                input += buttonText;
        }
        display.setText(input.isEmpty() ? "0" : input);
    }

    // Simple evaluation using stacks for +, -, *, /, %
    private double evaluate(String expr) throws Exception {
        expr = expr.replace("×", "*").replace("÷", "/").replace("%", "/100");
        return evalExpression(expr);
    }

    private double evalExpression(String expr) throws Exception {
        // Remove spaces
        expr = expr.replaceAll(" ", "");
        char[] tokens = expr.toCharArray();

        Stack<Double> values = new Stack<>();
        Stack<Character> ops = new Stack<>();
        for (int i = 0; i < tokens.length; i++) {
            if (tokens[i] >= '0' && tokens[i] <= '9' || tokens[i]=='.') {
                StringBuilder sbuf = new StringBuilder();
                while (i < tokens.length && (tokens[i] >= '0' && tokens[i] <= '9' || tokens[i]=='.'))
                    sbuf.append(tokens[i++]);
                i--;
                values.push(Double.parseDouble(sbuf.toString()));
            } else if (tokens[i] == '+' || tokens[i] == '-' || tokens[i] == '*' || tokens[i] == '/') {
                while (!ops.empty() && hasPrecedence(tokens[i], ops.peek()))
                    values.push(applyOp(ops.pop(), values.pop(), values.pop()));
                ops.push(tokens[i]);
            }
        }
        while (!ops.empty())
            values.push(applyOp(ops.pop(), values.pop(), values.pop()));

        return values.pop();
    }

    private boolean hasPrecedence(char op1, char op2) {
        if ((op2 == '*' || op2 == '/') && (op1 == '+' || op1 == '-'))
            return true;
        else
            return (op2 != '(' && op2 != ')');
    }

    private double applyOp(char op, double b, double a) throws Exception {
        switch (op) {
            case '+': return a + b;
            case '-': return a - b;
            case '*': return a * b;
            case '/': if (b == 0) throw new Exception("Division by zero"); return a / b;
        }
        return 0;
    }
}
```
activity_main.xml

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp"
    android:background="#F4F6F9">

    <TextView
        android:id="@+id/display"
        android:layout_width="match_parent"
        android:layout_height="100dp"
        android:background="#FFFFFF"
        android:text="0"
        android:textSize="36sp"
        android:textColor="#000000"
        android:gravity="end|center_vertical"
        android:padding="16dp"
        android:layout_marginBottom="16dp" />


    <GridLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:columnCount="4"
        android:rowCount="5"
        android:alignmentMode="alignMargins"
        android:rowOrderPreserved="false"
        android:columnOrderPreserved="false">

        <!-- Row 1 -->
        <Button android:id="@+id/btnC" android:text="C" android:layout_width="0dp" android:layout_height="80dp" android:layout_columnWeight="1" android:layout_margin="4dp" android:backgroundTint="#6200EE" android:textColor="#FFFFFF" android:textSize="24sp"/>
        <Button android:id="@+id/btnDiv" android:text="÷" android:layout_width="0dp" android:layout_height="80dp" android:layout_columnWeight="1" android:layout_margin="4dp" android:backgroundTint="#6200EE" android:textColor="#FFFFFF" android:textSize="24sp"/>
        <Button android:id="@+id/btnMul" android:text="×" android:layout_width="0dp" android:layout_height="80dp" android:layout_columnWeight="1" android:layout_margin="4dp" android:backgroundTint="#6200EE" android:textColor="#FFFFFF" android:textSize="24sp"/>
        <Button android:id="@+id/btnBack" android:text="⌫" android:layout_width="0dp" android:layout_height="80dp" android:layout_columnWeight="1" android:layout_margin="4dp" android:backgroundTint="#6200EE" android:textColor="#FFFFFF" android:textSize="24sp"/>

        <!-- Row 2 -->
        <Button android:id="@+id/btn7" android:text="7" android:layout_width="0dp" android:layout_height="80dp" android:layout_columnWeight="1" android:layout_margin="4dp"/>
        <Button android:id="@+id/btn8" android:text="8" android:layout_width="0dp" android:layout_height="80dp" android:layout_columnWeight="1" android:layout_margin="4dp"/>
        <Button android:id="@+id/btn9" android:text="9" android:layout_width="0dp" android:layout_height="80dp" android:layout_columnWeight="1" android:layout_margin="4dp"/>
        <Button android:id="@+id/btnSub" android:text="-" android:layout_width="0dp" android:layout_height="80dp" android:layout_columnWeight="1" android:layout_margin="4dp" android:backgroundTint="#6200EE" android:textColor="#FFFFFF" android:textSize="24sp"/>

        <!-- Row 3 -->
        <Button android:id="@+id/btn4" android:text="4" android:layout_width="0dp" android:layout_height="80dp" android:layout_columnWeight="1" android:layout_margin="4dp"/>
        <Button android:id="@+id/btn5" android:text="5" android:layout_width="0dp" android:layout_height="80dp" android:layout_columnWeight="1" android:layout_margin="4dp"/>
        <Button android:id="@+id/btn6" android:text="6" android:layout_width="0dp" android:layout_height="80dp" android:layout_columnWeight="1" android:layout_margin="4dp"/>
        <Button android:id="@+id/btnAdd" android:text="+" android:layout_width="0dp" android:layout_height="80dp" android:layout_columnWeight="1" android:layout_margin="4dp" android:backgroundTint="#6200EE" android:textColor="#FFFFFF" android:textSize="24sp"/>

        <!-- Row 4 -->
        <Button android:id="@+id/btn1" android:text="1" android:layout_width="0dp" android:layout_height="80dp" android:layout_columnWeight="1" android:layout_margin="4dp"/>
        <Button android:id="@+id/btn2" android:text="2" android:layout_width="0dp" android:layout_height="80dp" android:layout_columnWeight="1" android:layout_margin="4dp"/>
        <Button android:id="@+id/btn3" android:text="3" android:layout_width="0dp" android:layout_height="80dp" android:layout_columnWeight="1" android:layout_margin="4dp"/>
        <Button android:id="@+id/btnEqual" android:text="=" android:layout_width="0dp" android:layout_height="160dp" android:layout_columnWeight="1" android:layout_margin="4dp" android:backgroundTint="#03DAC5" android:textColor="#FFFFFF" android:textSize="24sp"/>

        <!-- Row 5 -->
        <Button android:id="@+id/btn0" android:text="0" android:layout_width="0dp" android:layout_height="80dp" android:layout_columnSpan="2" android:layout_margin="4dp"/>
        <Button android:id="@+id/btnDot" android:text="." android:layout_width="0dp" android:layout_height="80dp" android:layout_columnWeight="1" android:layout_margin="4dp"/>
        <Button android:id="@+id/btnPercent" android:text="%" android:layout_width="0dp" android:layout_height="80dp" android:layout_columnWeight="1" android:layout_margin="4dp"/>

    </GridLayout>
</LinearLayout>
```

## OUTPUT:

![WhatsApp Image 2025-10-21 at 23 48 40_34607dbe](https://github.com/user-attachments/assets/2c82853f-216b-4234-919c-e348d3b112ce)

![WhatsApp Image 2025-10-21 at 23 49 02_d8831d19](https://github.com/user-attachments/assets/f8a5abe0-7066-440c-9b20-63b1ba7675c0)

## RESULT:
Thus a Simple Android Application to create a calculator using Android Studio was developed and executed successfully.


