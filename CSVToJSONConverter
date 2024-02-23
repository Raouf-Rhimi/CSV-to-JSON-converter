import java.io.*;
import java.util.List;
import java.util.Map;
import com.fasterxml.jackson.databind.MappingIterator;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.dataformat.csv.CsvMapper;
import com.fasterxml.jackson.dataformat.csv.CsvSchema;

public class CSVToJSONConverter {

    /**
     * This method converts the list of CSV files into a list of JSON files
     */
    public void convertCSVToJSON(){
        String[] files = {"file1","file2"}; // List of csv files to be converted
        for(String file :files){
            InputStream source = CSVToJSONConverter.class.getClassLoader().getResourceAsStream("source/"+file+".csv");
            if (source == null) {
                System.err.println("Could not find input file.");
                return;
            }
            try {
                List<Map<?, ?>> data = readObjectsFromCsv(source);
                writeAsJson(data,file);
                System.out.println("Conversion completed successfully.");
            }catch (Exception e){
                System.out.println(e.getMessage());
            }
        }

    }

    /**
     * This method reads the input stream and return it as a Map
     * @param inputStream
     * @return
     * @throws IOException
     */

    public  List<Map<?, ?>> readObjectsFromCsv(InputStream inputStream) throws IOException {
        CsvSchema bootstrap = CsvSchema.emptySchema().withHeader();
        CsvMapper csvMapper = new CsvMapper();
        try (MappingIterator<Map<?, ?>> mappingIterator = csvMapper.readerFor(Map.class).with(bootstrap).readValues(inputStream)) {
            return mappingIterator.readAll();
        }
    }


    /**
     * This method takes a Map and convert it to JSON file
     * @param data
     * @param filename
     */
    public  void writeAsJson(List<Map<?, ?>> data,String filename){
        ObjectMapper mapper = new ObjectMapper();
        File outputDir = new File("src/main/resources/target");
        if (!outputDir.exists()) {
            outputDir.mkdirs();
        }
        File outputFile = new File(outputDir, filename+".json");
        try {
            mapper.writerWithDefaultPrettyPrinter().writeValue(outputFile, data);
        } catch (Exception ex) {
            throw new RuntimeException("Error writing JSON", ex);
        }
    }

}
