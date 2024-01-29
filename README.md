# calculator
import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {
    private TextView resultTextView;
    private String operand = "";
    private String operator = "";
    private double firstOperand = Double.NaN;

    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        resultTextView = findViewById(R.id.resultTextView);

    
        int[] digitButtonIds = {R.id.button0, R.id.button1, R.id.button2, R.id.button3, R.id.button4,
                R.id.button5, R.id.button6, R.id.button7, R.id.button8, R.id.button9};
        for (int id : digitButtonIds) {
            findViewById(id).setOnClickListener(this);
        }

      
        int[] operatorButtonIds = {R.id.buttonAdd, R.id.buttonSubtract, R.id.buttonMultiply, R.id.buttonDivide, R.id.buttonEquals};
        for (int id : operatorButtonIds) {
            findViewById(id).setOnClickListener(this);
        }

      
        Button clearButton = findViewById(R.id.buttonClear);
        clearButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                clear();
            }
        });
    }


    public void onClick(View v) {
        Button button = (Button) v;
        String buttonText = button.getText().toString();

        switch (v.getId()) {
            case R.id.buttonEquals:
                calculateResult();
                break;
            case R.id.buttonClear:
                clear();
                break;
            default:
                if (buttonText.matches("[0-9]")) { // If clicked button is a digit
                    operand += buttonText;
                    resultTextView.setText(operand);
                } else { // If clicked button is an operator
                    operator = buttonText;
                    if (!Double.isNaN(firstOperand)) {
                        calculateResult();
                    }
                    firstOperand = Double.parseDouble(operand);
                    operand = "";
                }
                break;
        }
    }

    private void calculateResult() {
        if (!operand.isEmpty()) {
            double secondOperand = Double.parseDouble(operand);
            double result = 0;
            switch (operator) {
                case "+":
                    result = firstOperand + secondOperand;
                    break;
                case "-":
                    result = firstOperand - secondOperand;
                    break;
                case "*":
                    result = firstOperand * secondOperand;
                    break;
                case "/":
                    if (secondOperand != 0) {
                        result = firstOperand / secondOperand;
                    } else {
                        resultTextView.setText("Error");
                        return;
                    }
                    break;
            }
            resultTextView.setText(String.valueOf(result));
            operand = String.valueOf(result);
            firstOperand = result;
        }
    }

    private void clear() {
        operand = "";
        operator = "";
        firstOperand = Double.NaN;
        resultTextView.setText("");
    }
}


xml code
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/resultTextView"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="32dp"
        android:textAlignment="center"
        android:textSize="24sp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        tools:text="0" />

    <Button
        android:id="@+id/buttonClear"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="C"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/resultTextView" />

    <Button
        android:id="@+id/buttonDivide"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:text="/"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/buttonClear" />

  

</androidx.constraintlayout.widget.ConstraintLayout>
