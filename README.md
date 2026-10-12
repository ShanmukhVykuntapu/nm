import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.ComboBox;
import javafx.scene.control.Label;
import javafx.scene.control.TextField;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class TipCalculator extends Application {

    @Override
    public void start(Stage stage) {

        // Heading
        Label title = new Label("Tip Calculator");
        title.setStyle("-fx-font-size: 24px; -fx-font-weight: bold;");

        // Bill amount
        Label billLabel = new Label("Enter Bill Amount (₹):");
        TextField billField = new TextField();
        billField.setPromptText("Enter bill amount");

        // Tip percentage
        Label tipLabel = new Label("Select Tip Percentage:");
        ComboBox<Integer> tipBox = new ComboBox<>();
        tipBox.getItems().addAll(5, 10, 15, 20, 25);
        tipBox.setValue(10);

        // Calculate button
        Button calculateButton = new Button("Calculate Tip");

        // Result labels
        Label tipResult = new Label("Tip Amount: ₹0.00");
        Label totalResult = new Label("Total Bill: ₹0.00");
        Label errorLabel = new Label();

        // Button event handling
        calculateButton.setOnAction(event -> {
            try {
                double bill = Double.parseDouble(billField.getText());
                int percentage = tipBox.getValue();

                if (bill < 0) {
                    errorLabel.setText("Please enter a positive amount.");
                    tipResult.setText("Tip Amount: ₹0.00");
                    totalResult.setText("Total Bill: ₹0.00");
                    return;
                }

                double tip = bill * percentage / 100.0;
                double total = bill + tip;

                tipResult.setText(
                    String.format("Tip Amount: ₹%.2f", tip)
                );

                totalResult.setText(
                    String.format("Total Bill: ₹%.2f", total)
                );

                errorLabel.setText("");

            } catch (NumberFormatException e) {
                errorLabel.setText("Please enter a valid bill amount.");
                tipResult.setText("Tip Amount: ₹0.00");
                totalResult.setText("Total Bill: ₹0.00");
            }
        });

        // Clear button
        Button clearButton = new Button("Clear");

        clearButton.setOnAction(event -> {
            billField.clear();
            tipBox.setValue(10);
            tipResult.setText("Tip Amount: ₹0.00");
            totalResult.setText("Total Bill: ₹0.00");
            errorLabel.setText("");
        });

        // Layout
        VBox root = new VBox(12);
        root.setPadding(new Insets(20));
        root.setAlignment(Pos.CENTER);

        root.getChildren().addAll(
            title,
            billLabel,
            billField,
            tipLabel,
            tipBox,
            calculateButton,
            tipResult,
            totalResult,
            errorLabel,
            clearButton
        );

        Scene scene = new Scene(root, 350, 450);

        stage.setTitle("Tip Calculator");
        stage.setScene(scene);
        stage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
