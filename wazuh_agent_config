#On the wazuh agent
#vim /var/ossec/active-response/bin/yara.sh

#!/bin/bash
#------------------------- Gather parameters -------------------------#

# Static active response parameters
FILENAME=$8
LOCAL=`dirname $0`

# Extra arguments
YARA_PATH=/opt/yara-4.1.0/  # where the yara installed
YARA_RULES=/opt/yara-4.1.0/rules/index.yar # where the yara installed

while [ "$1" != "" ]; do
  case $1 in
    -yara_path)       shift
                      YARA_PATH=$1
                      ;;
    -yara_rules)      shift
                      YARA_RULES=$1
                      ;;
    * )               shift
  esac
  shift
done

# Move to the active response folder
cd $LOCAL
cd ../

# Set LOG_FILE path
PWD=`pwd`
LOG_FILE="${PWD}/../logs/active-responses.log"

#----------------------- Analyze parameters -----------------------#

if [[ ! $YARA_PATH ]] || [[ ! $YARA_RULES ]]
then
    echo "wazuh-yara: error: Yara path and rules parameters are mandatory." >> ${LOG_FILE}
    exit
fi


#------------------------- Main workflow --------------------------#

# Execute YARA scan on the specified filename
yara_output=$(${YARA_PATH}/yara -w -r $YARA_RULES $FILENAME)


if [[ $yara_output != "" ]]
then
    # Iterate every detected rule and append it to the LOG_FILE
    while read -r line; do
        echo "wazuh-yara: info: $line" >> ${LOG_FILE}
    done <<< "$yara_output"
fi

exit 1;
~
