as_site_config = {
  # For SIT, add cache clearing
  app_command_line = "rm -rf node_modules .angular && npm cache clean --force && npm install && npm run build && npx serve -s dist/wpui/browser -l $PORT"
}


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




   public RedisTemplate<String, List<Student>> studentRedisTemplate(
            RedisConnectionFactory connectionFactory) {
        
        RedisTemplate<String, List<Student>> template = new RedisTemplate<>();
        template.setConnectionFactory(connectionFactory);
        
        // Create ObjectMapper
        ObjectMapper objectMapper = new ObjectMapper();
        
        // Create serializer for List<Student> specifically
        JavaType listType = objectMapper.getTypeFactory()
            .constructCollectionType(List.class, Student.class);
        
        Jackson2JsonRedisSerializer<List<Student>> serializer = 
            new Jackson2JsonRedisSerializer<>(objectMapper, listType);
        
        template.setKeySerializer(new StringRedisSerializer());
        template.setValueSerializer(serializer);
        
        template.afterPropertiesSet();
        return template;
