# PDF to XLSX Conversion - CIE & SEE Marks Improvements

## Summary of Changes

Enhanced the PDF to Excel conversion process to properly extract, validate, and display CIE (Continuous Internal Evaluation) and SEE (Semester End Examination) marks.

## Key Improvements

### 1. **Enhanced Column Detection** (`getColumnPositions` function)
- Added detection for "CIE" and "Continuous" keywords to identify internal marks columns
- Added detection for "SEE" and "Semester" keywords to identify external marks columns
- Improved header line matching with more flexible patterns

### 2. **Smart Mark Extraction** (`parseSubjectChunk` function)
- Improved logic to distinguish between CIE marks (0-50) and SEE marks (0-50)
- Added validation to detect if marks are in correct order or swapped
- CIE marks validation: expects values between 0-50
- SEE marks validation: expects values between 0-50
- Total calculation: CIE + SEE should equal total (0-100)

### 3. **Enhanced Row Parsing** (`extractMarksFromRowText` function)
- Smart detection of mark ranges to identify CIE vs SEE marks
- Automatic correction if marks appear to be in reversed order
- Handles missing marks gracefully

### 4. **Mark Validation and Correction** (`parseVtuTable` function)
- Added validation after extraction to ensure CIE ≤ 50 and SEE ≤ 50
- Automatic swap correction if SEE appears smaller than CIE
- Ensures total is properly calculated

### 5. **Excel Sheet Headers**
- Updated column headers to show: "CIE Marks (Out of 50)" and "SEE Marks (Out of 50)"
- Added maximum marks information for clarity in student list
- Headers now show: `{CODE}_CIE (50)` and `{CODE}_SEE (50)`

### 6. **Multi-Sheet Workbooks**
Now generates Excel files with multiple sheets:
- **Result Template**: Shows academic year, branch, semester with CIE and SEE structure
- **Student Marks**: Combined view of all students with mark totals
- **PDF Detailed Data**: Complete breakdown with all marks clearly labeled
- **Subject Marks**: Detailed subject-wise marks with CIE, SEE, Total, and Result
- **Course Details**: Subject codes with max marks (CIE: 50, SEE: 50, Total: 100)
- **Student List**: Individual student performance with CIE, SEE, Total, and Result per subject

### 7. **Improved Status Messages**
- Added detailed feedback about what was extracted
- Shows mark format: "CIE (Out of 50) + SEE (Out of 50) = Total (Out of 100)"
- Includes subject count and student information

### 8. **OCR Support**
- Enhanced OCR-based extraction to properly identify CIE and SEE columns
- Better handling of scanned PDFs and image-based documents

## Extraction Workflow

1. **PDF Reading**: Extract text and position data from PDF
2. **Header Detection**: Identify columns including CIE and SEE headers
3. **Row Parsing**: Extract marks while validating ranges
4. **Mark Validation**: Ensure CIE (0-50), SEE (0-50), Total (0-100)
5. **Auto-Correction**: Swap marks if order appears reversed
6. **Excel Generation**: Create multi-sheet workbook with clear labeling

## Mark Ranges

- **CIE Marks**: 0 to 50 (Continuous Internal Evaluation)
- **SEE Marks**: 0 to 50 (Semester End Examination)
- **Total**: 0 to 100 (CIE + SEE)

## Supported PDF Formats

1. **VTU Format**: Standard VTU result transcripts
2. **Table-based**: PDFs with structured tables
3. **Image-based**: Scanned PDFs (using OCR)
4. **Multi-student**: PDFs with multiple students on one page/file

## Excel Output Features

- Clearly labeled CIE and SEE columns
- Automatic total calculation
- Pass/Fail determination (Pass: CIE ≥ 20 AND SEE ≥ 18)
- Academic information (Year, Semester, Branch)
- Date announced (if available)
- Multi-sheet organization for different analysis types

## Error Handling

- Validates mark ranges automatically
- Detects and corrects reversed mark columns
- Handles missing data gracefully
- Falls back to OCR for image-based PDFs
- Provides clear error messages if extraction fails

## Testing Recommendations

1. Test with standard VTU result PDFs
2. Test with scanned result documents
3. Verify CIE marks are in 0-50 range
4. Verify SEE marks are in 0-50 range
5. Check total calculations (CIE + SEE = Total)
6. Verify pass/fail logic (both CIE ≥ 20 AND SEE ≥ 18)
