import java.text.Normalizer;

public static String normalizeMessage(String input) {
    // Step 1: Normalize to decompose accented letters (é → e + ́)
    String normalized = Normalizer.normalize(input, Normalizer.Form.NFD);

    // Step 2: Remove combining diacritical marks (the accents like ́)
    normalized = normalized.replaceAll("\\p{InCombiningDiacriticalMarks}+", "");

    // Step 3: Replace common French and Unicode special characters with ASCII
    normalized = normalized
        .replace("–", "-")   // en dash to dash
        .replace("—", "-")   // em dash to dash
        .replace("’", "'")   // right single quote to apostrophe
        .replace("‘", "'")   // left single quote to apostrophe
        .replace("“", "\"")  // left double quote to quote
        .replace("”", "\"")  // right double quote to quote
        .replace("œ", "oe")  // ligature to plain letters
        .replace("Œ", "OE")  // uppercase version
        .replace("€", "EUR") // euro symbol to "EUR"
        .replace("«", "\"")  // French opening quote
        .replace("»", "\""); // French closing quote

    return normalized;
}
