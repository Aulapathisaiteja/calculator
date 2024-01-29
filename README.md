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

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        resultTextView = findViewById(R.id.resultTextView);

        // Set OnClickListener for all digit buttons
        int[] digitButtonIds = {R.id.button0, R.id.button1, R.id.button2, R.id.button3, R.id.button4,
                R.id.button5, R.id.button6, R.id.button7, R.id.button8, R.id.button9};
        for (int id : digitButtonIds) {
            findViewById(id).setOnClickListener(this);
        }

        // Set OnClickListener for operator buttons
        int[] operatorButtonIds = {R.id.buttonAdd, R.id.buttonSubtract, R.id.buttonMultiply, R.id.buttonDivide, R.id.buttonEquals};
        for (int id : operatorButtonIds) {
            findViewById(id).setOnClickListener(this);
        }

        // Clear button
        Button clearButton = findViewById(R.id.buttonClear);
        clearButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                clear();
            }
        });
    }

    @Override
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
