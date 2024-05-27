# Task-1
import javafx.application.Application;
import javafx.fxml.FXMLLoader;
import javafx.fxml.FXML;
import javafx.scene.Scene;
import javafx.scene.control.MenuItem;
import javafx.scene.control.TextField;
import javafx.scene.control.TabPane;
import javafx.scene.layout.VBox;
import javafx.scene.layout.BorderPane;
import javafx.scene.web.WebView;
import javafx.stage.Stage;
import com.itextpdf.text.Document;
import com.itextpdf.text.DocumentException;
import com.itextpdf.text.Paragraph;
import com.itextpdf.text.pdf.PdfWriter;
import org.apache.poi.xwpf.usermodel.XWPFDocument;
import org.apache.poi.xwpf.usermodel.XWPFParagraph;
import org.apache.poi.xwpf.usermodel.XWPFRun;

import java.io.FileOutputStream;
import java.io.IOException;

public class ResumeBuilderApp extends Application {

    @Override
    public void start(Stage primaryStage) throws Exception {
        FXMLLoader loader = new FXMLLoader(getClass().getResource("main.fxml"));
        primaryStage.setTitle("Resume Builder");
        primaryStage.setScene(new Scene(loader.load()));
        primaryStage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }

    public static class ResumeBuilderController {
        @FXML private TextField nameField;
        @FXML private TextField emailField;
        @FXML private WebView previewWebView;

        @FXML
        private void handleNew() {
            clearForm();
        }

        @FXML
        private void handleSave() {
            // Implement save functionality
        }

        @FXML
        private void handleOpen() {
            // Implement open functionality
        }

        @FXML
        private void handleExportPDF() {
            exportToPDF(nameField.getText(), emailField.getText());
        }

        @FXML
        private void handleExportWord() {
            exportToWord(nameField.getText(), emailField.getText());
        }

        @FXML
        private void handleExit() {
            System.exit(0);
        }

        private void clearForm() {
            nameField.clear();
            emailField.clear();
            // Clear other fields as necessary
        }

        private void exportToPDF(String name, String email) {
            Document document = new Document();
            try {
                PdfWriter.getInstance(document, new FileOutputStream("Resume.pdf"));
                document.open();
                document.add(new Paragraph("Name: " + name));
                document.add(new Paragraph("Email: " + email));
                // Add other details
                document.close();
            } catch (DocumentException | IOException e) {
                e.printStackTrace();
            }
        }

        private void exportToWord(String name, String email) {
            XWPFDocument document = new XWPFDocument();
            try (FileOutputStream out = new FileOutputStream("Resume.docx")) {
                XWPFParagraph paragraph = document.createParagraph();
                XWPFRun run = paragraph.createRun();
                run.setText("Name: " + name);
                run.addBreak();
                run.setText("Email: " + email);
                // Add other details
                document.write(out);
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}

// FXML Layout - main.fxml
// Save this XML content as "main.fxml" in the same directory as the Java file.
/*
<?xml version="1.0" encoding="UTF-8"?>

<?import javafx.scene.control.*?>
<?import javafx.scene.layout.*?>

<BorderPane xmlns:fx="http://javafx.com/fxml" fx:controller="ResumeBuilderApp$ResumeBuilderController">
    <top>
        <MenuBar>
            <Menu text="File">
                <MenuItem text="New" onAction="#handleNew"/>
                <MenuItem text="Open" onAction="#handleOpen"/>
                <MenuItem text="Save" onAction="#handleSave"/>
                <MenuItem text="Export as PDF" onAction="#handleExportPDF"/>
                <MenuItem text="Export as Word" onAction="#handleExportWord"/>
                <SeparatorMenuItem />
                <MenuItem text="Exit" onAction="#handleExit"/>
            </Menu>
        </MenuBar>
    </top>
    <center>
        <TabPane>
            <Tab text="Personal Information">
                <VBox>
                    <TextField promptText="Name" fx:id="nameField"/>
                    <TextField promptText="Email" fx:id="emailField"/>
                    <!-- Add more fields as necessary -->
                </VBox>
            </Tab>
            <Tab text="Work Experience">
                <!-- Add UI components for work experience -->
            </Tab>
            <Tab text="Education">
                <!-- Add UI components for education -->
            </Tab>
            <Tab text="Skills">
                <!-- Add UI components for skills -->
            </Tab>
            <Tab text="Preview">
                <WebView fx:id="previewWebView"/>
            </Tab>
        </TabPane>
    </center>
</BorderPane>
*/
