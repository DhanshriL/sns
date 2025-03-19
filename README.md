# sns
import boto3
import json

sns_client = boto3.client('sns')
SNS_TOPIC_ARN = 'arn:aws:sns:us-east-1:123456789012:StockAlerts'

def lambda_handler(event, context):
    stock_symbol = event['stock_symbol']
    price = float(event['price'])
    threshold = 60.0

    if price > threshold:
        message = {
            "Stock": stock_symbol,
            "Price": price,
            "Message": "Price exceeded threshold!"
        }
        sns_client.publish(
            TopicArn=SNS_TOPIC_ARN,
            Message=json.dumps(message),
            Subject="Stock Price Alert"
        )
        return {"status": "Notification sent"}
    else:
        return {"status": "Price within limit"}
