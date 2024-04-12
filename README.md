# Umfrage-Webanwendung
Benutzer können Fragen beantworten, indem sie Textfelder ausfüllen und dann ihre Antworten absenden. Die Antworten werden in einer Textdatei gespeichert (answers.txt). Das Projekt verwendet nur PHP und HTML und benötigt keine Datenbank.
<?php
// Speichern der Umfragefragen und Antworten in einem Array
$questions = array(
    "1. Wie zufrieden sind Sie mit unserem Service?",
    "2. Welches Produkt würden Sie gerne in Zukunft sehen?",
    "3. Wie wahrscheinlich ist es, dass Sie uns weiterempfehlen?",
);

// Überprüfen, ob das Formular abgesendet wurde
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    // Überprüfen, ob alle Fragen beantwortet wurden
    $all_answered = true;
    foreach ($questions as $question) {
        $answer = $_POST[$question];
        if (empty($answer)) {
            $all_answered = false;
            break;
        }
    }
    // Wenn alle Fragen beantwortet wurden, die Antworten speichern
    if ($all_answered) {
        $answers_file = 'answers.txt';
        $file_content = "";
        foreach ($questions as $question) {
            $answer = $_POST[$question];
            $file_content .= "$question: $answer\n";
        }
        // Antworten in einer Textdatei speichern
        file_put_contents($answers_file, $file_content, FILE_APPEND);
        echo "Vielen Dank für Ihre Teilnahme an der Umfrage!";
        exit();
    } else {
        echo "Bitte beantworten Sie alle Fragen.";
    }
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Umfrage</title>
</head>
<body>
    <h1>Umfrage</h1>
    <form method="post">
        <?php foreach ($questions as $question): ?>
            <p><?php echo $question; ?></p>
            <input type="text" name="<?php echo $question; ?>" required><br><br>
        <?php endforeach; ?>
        <button type="submit">Absenden</button>
    </form>
</body>
</html>
