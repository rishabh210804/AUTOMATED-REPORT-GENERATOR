# AUTOMATED-REPORT-GENERATOR
import pandas as pd
from reportlab.lib.pagesizes import A4
from reportlab.pdfgen import canvas
from datetime import datetime

# Step 1: Read data from CSV
def read_data(file_path):
    try:
        data = pd.read_csv(file_path)
        return data
    except Exception as e:
        print(f"Error reading file: {e}")
        return None

# Step 2: Analyze data
def analyze_data(data):
    summary = data.describe(include='all')
    return summary

# Step 3: Generate PDF report
def generate_pdf_report(data_summary, output_path):
    c = canvas.Canvas(output_path, pagesize=A4)
    width, height = A4

    # Title
    c.setFont("Helvetica-Bold", 16)
    c.drawString(100, height - 50, "Automated Data Analysis Report")

    # Date
    c.setFont("Helvetica", 10)
    c.drawString(100, height - 70, f"Generated on: {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}")

    # Body
    c.setFont("Helvetica", 10)
    text_obj = c.beginText(50, height - 100)
    text_obj.setLeading(14)

    for col in data_summary.columns:
        text_obj.textLine(f"Column: {col}")
        for index, value in data_summary[col].items():
            text_obj.textLine(f"  {index}: {value}")
        text_obj.textLine("")

    c.drawText(text_obj)
    c.save()

# Main
if __name__ == "__main__":
    input_csv = "data.csv"          # Replace with your file
    output_pdf = "sample_report.pdf"

    df = read_data(input_csv)
    if df is not None:
        summary = analyze_data(df)
        generate_pdf_report(summary, output_pdf)
        print(f"Report generated: {output_pdf}")
