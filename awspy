#!/Users/patrickwilliamson/anaconda3/bin/python

import boto3
import sys
import configparser

def get_id():
    instance = configparser.ConfigParser()
    instance.read('../config/aws/instance')
    id = instance['Info']['id']
    return id

def check_status(ec2):
    id = get_id()
    response = ec2.describe_instances(InstanceIds=[id])
    status = response['Reservations'][0]['Instances'][0]['State']['Name']

    if status == 'stopped':
        return 0
    if status == 'running':
        return 1
    if status == 'stopping':
        return 2
    if status == 'starting':
        return 3

def start(ec2):
    response = ec2.start_instances(InstanceIds=[get_id()])
    if 'StartingInstances' in response.keys():
        print('Starting instance')
    else:
        print('Starting intance failed')

def stop(ec2):
    ec2.stop_instances(InstanceIds=[get_id()])
    print('Stopping instance')

def main():
    ec2 = boto3.client('ec2')
    status = check_status(ec2)
    
    if len(sys.argv) == 1:
        print('missing arg')

    elif sys.argv[1] == 'start':
        if status == 1:
            print('Instance already running')
        else:
            start(ec2)

    elif sys.argv[1] == 'stop':
        if status == 0:
            print('Instance already stopped')
        else:
            stop(ec2)

    elif sys.argv[1] == 'status':
        if status == 0:
            print('Instance is stopped')
        if status == 1:
            print('Instance is running')

if __name__ == '__main__':
    main()

