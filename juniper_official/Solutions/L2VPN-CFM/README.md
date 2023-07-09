# HealthBot CFM KPI rules and playbooks for L2VPN

## CFM playbooks

### Playbook name: cfm-interface-status-playbook 
        	> Description: "Playbook to monitor CFM interface, link and remote MEP status"

        	> Playbook file name: cfm-interface-status.playbook
        	> Details:
              Playbook to monitor CFM interface, link and remote MEP status and notify
              anomalies when status' are not nromal.
        	
        	  1) Rule cfm-interface-status-netconf, Device playbook to monitor CFM interface, link and remote MEP status";
                 synopsis "Customized CFM interface, link and remote MEP status.

### Playbook name: cfm-dm-statistics-playbook
        	> Description: "Device playbook to monitor CFM twoway, forward and backward delay?"

        	> Playbook file name: cfm-dm-statistics.playbook
        	> Details:
              Playbook monitor CFM average, min and max twoway delay statistics and
              average forward and backward delay statistics  using two-way-delay
              iterator profile and notify in  case delay is more than static threshold limit.
        	
        	  1) Rule check-cfm-dm-statistics-netconf, imonitor average, min, max twoway delay
                 and avergae forward and backward delay and notifies
                 anomalies when delay exceeds static thresholds.

### Playbook name: cfm-slm-statistics-playbook 
        	> Description: "Device playbook to monitor CFM Synthetic Loss Measurment(SLM) statistics"

        	> Playbook file name: cfm-slm-statistics.playbook
        	> Details:
              Playbook monitor CFM forward and backward frame loss statistics based on slm iterator profile and notify in
              case frame loss is more than static threshold limit.
        	
        	  1) Rule check-slm-delay-statistics-netconf, monitor forward and backward frame loss using slm iterator profile and notifies
              anomalies when frame loss  exceeds static thresholds.

### Playbook name: cfm-all-statistics-playbook 
        	> Description: "Device playbook to monitor CFM delay"

        	> Playbook file name: cfm-all-statistics.playbook
        	> Details:
              All-in-one Playbook to monitor 
                - CFM interface, link and remote MEP status,
                - CFM average, min and max for forward, backward and twoway delays and
                - CFM forward and backward loss measurment(SLM) 
              and notifies anomalies. 
 
              1) Rule cfm-interface-status-netconf, Monitors CFM interface status and notify anomalies related to link status and remote mep state and notifies anomalies
                 if link status and remote MEP state are not normal.
       
              2) Rule check-cfm-dm-statistics-netconf, Monitors CFM average, min and max delay for forward, backward and twoway for configured destination and notifies anomalies
                 when average, min and max for forward, backward and twoway delays are above correspnding static thresholds.
   
              3) Rule check-cfm-slm-statistics-netconf, monitor CFM forward and backward frame loss to configured destination and notifies anomalies
                 when forward and backward frame loss is above static thresholds.

### Playbook name: l2svc-l2ckt-vpls-playbook 
        	> Description: "Monitors L2 circuit and VPLS connection status"

        	> Playbook file name: l2svc-l2ckt-vpls-conn-status.playbook
        	> Details:
              Monitors L2 circuit and VPLS connection status and notify anomalies.

              1) Rule check-l2svc-l2ckt-conn-status, Monitors L2Circuit connction  status and notify anomalies
    
              2) Rule check-l2svc-vpls-conn-status , Monitors VPLS service connction  status and notify anomalies
          

## CFM rules

### Rule name: cfm-interface-status-netconf
        	> Description: "isplay CFM interface status and raise evnet if remote state is not ok"
        	> Synopsis: "CFM interface info KPI"
        	> Rule file name: check-cfm-interface-status-netconf.rule
        	> Sensor type: sensor cfm_iAgent 
        	> Supported HealthBot version: 4.0.0
        	> Supported product:MX, Platforms:A, Junos:18.1R1


        	> Helper files: cfm-if.yml;
        	> More details:
          Monitors CFM interface status and notify anomalies related to link status and
          remote mep state and notifies anomalies
          if link status and remote MEP state are not normal.
          Three inputs control CFM interface selection:
            1) cfm-if-name, CFM interface name in regex "
            2) cfm-local-mep-id, CFM local mep name in regex
            1) cfm-remote-mep-id, FM remote mep name in regex
         Minimum sample configuration required in Junos MX configuration
          set interfaces ge-0/3/9 description "Connect to lab-asr9k10-02 gi0/0/1/19"
          set interfaces ge-0/3/9 unit 0 family inet address 10.3.19.1/30
          set protocols oam ethernet connectivity-fault-management performance-monitoring enhanced-sla-iterator
          set protocols oam ethernet connectivity-fault-management performance-monitoring measurement-interval 5
          set protocols oam ethernet connectivity-fault-management maintenance-domain PCORE level 0
          set protocols oam ethernet connectivity-fault-management maintenance-domain PCORE maintenance-association PCORE continuity-check interval 100ms
          set protocols oam ethernet connectivity-fault-management maintenance-domain PCORE maintenance-association PCORE mep 100 interface ge-0/3/9

### Rule name: check-cfm-dm-statistics-netconf 
        	> Description: "Monitors CFM average and max twoway, forward and backward delay to configured destination "
        	> Synopsis: "CFM delay statistics KPI"
        	> Rule file name: check-cfm-dm-statistics.rule
        	> Sensor type: sensor cfm_iAgent 
        	> Supported HealthBot version: 4.0.0
        	> Supported product:MX, Platforms:A, Junos:18.1R1


        	> Helper files: cfm-dm.yml;
        	> More details:
          Monitors CFM average, min and max for forward, backward and twoway delays for configured destination and notifies anomalies
          when average, min and max delay is above static thresholds for corresponding delay exceeds static thresholds.
          Following inputs control detection:
 
            1) maximum-twoway-delay-avg-threshold, is the threshold that causes the rule to report an
               anomaly. By default it is 100000 micro seconds. This rule will set a
               dashboard color to red when CFM average twoway delay time is greater than static
               threshold 'maximum-twoway-delay-avg-threshold-value'.
            2) maximum-twoway-delay-max-threshold, is the threshold that causes the rule to report an
               anomaly. By default it is 1000000 micro seconds. This rule will set a
               dashboard color to red when CFM max twoway delay time is greater than static
               threshold 'maximum-twoway-delay-max-threshold-value'.
            3) maximum-twoway-delay-min-threshold, is the threshold that causes the rule to report an
               anomaly. By default it is 1000000 micro seconds. This rule will set a
               dashboard color to red when CFM min twoway delay time is greater than static
               threshold 'maximum-twoway-delay-min-threshold-value'.
            4) maximum-forward-delay-avg-threshold, is the threshold that causes the rule to report an
               anomaly. By default it is 1000000 micro seconds. This rule will set a
               dashboard color to red when CFM average forward delay time is greater than static
               threshold 'maximum-forward-delay-avg-threshold'.
            4) maximum-backward-delay-avg-threshold, is the threshold that causes the rule to report an
               anomaly. By default it is 1000000 micro seconds. This rule will set a
               dashboard color to red when CFM average forward delay time is greater than static
               threshold 'maximum-backward-delay-avg-threshold'.
         Minimum sample configuration required in Junos MX configuration
          set interfaces ge-0/3/9 description "Connect to lab-asr9k10-02 gi0/0/1/19"
          set interfaces ge-0/3/9 unit 0 family inet address 10.3.19.1/30
          set protocols oam ethernet connectivity-fault-management performance-monitoring enhanced-sla-iterator
          set protocols oam ethernet connectivity-fault-management performance-monitoring measurement-interval 5
          set protocols oam ethernet connectivity-fault-management performance-monitoring sla-iterator-profiles PCORE measurement-type two-way-delay
          set protocols oam ethernet connectivity-fault-management performance-monitoring sla-iterator-profiles PCORE measurement-interval 5
          set protocols oam ethernet connectivity-fault-management maintenance-domain PCORE level 0
          set protocols oam ethernet connectivity-fault-management maintenance-domain PCORE maintenance-association PCORE continuity-check interval 100ms
          set protocols oam ethernet connectivity-fault-management maintenance-domain PCORE maintenance-association PCORE mep 100 interface ge-0/3/9
          set protocols oam ethernet connectivity-fault-management maintenance-domain PCORE maintenance-association PCORE mep 100 remote-mep 101 sla-iterator-profile PCORE

### Rule name: check-cfm-slm-statistics-netconf 
        	> Description: "Monitors CFM forward and backward frame loss to configured destination"
        	> Synopsis: "CFM forward and backward frame los statistics KPI"
        	> Rule file name: check-cfm-slm-statistics.rule
        	> Sensor type: sensor cfm_iAgent 
        	> Supported HealthBot version: 4.0.0
        	> Supported product:MX, Platforms:A, Junos:18.1R1


        	> Helper files: cfm-slm.yml;
        	> More details:
              Monitors CFM forward and backward frame loss to configured destination and notifies anomalies
              when forward and backward frame loss is above static thresholds.
              Two inputs control detection:
              1) maximum-forward-frame-loss-threshold, is the threshold that causes the rule to report an
                 anomaly. By default it is 100000. This rule will set a
                 dashboard color to red when CFM maximum forward frame loss is greater than static
                 threshold 'maximum-forward-frame-loss-threshold-value'.
                 This rule will set a dashboard color to yellow when forward frame loss is zero.
              2) maximum-backward-frame-loss-threshold, is the threshold that causes the rule to report an
                 anomaly. By default it is 1000000. This rule will set a
                 dashboard color to red when CFM maximum backward frame loss is greater than static
                 threshold 'maximum-backward-frame-loss-threshold-value'.
         Minimum sample configuration required in Junos MX configuration
          set interfaces ge-0/3/9 description "Connect to lab-asr9k10-02 gi0/0/1/19"
          set interfaces ge-0/3/9 unit 0 family inet address 10.3.19.1/30
          set protocols oam ethernet connectivity-fault-management performance-monitoring enhanced-sla-iterator
          set protocols oam ethernet connectivity-fault-management performance-monitoring measurement-interval 5
          set protocols oam ethernet connectivity-fault-management performance-monitoring sla-iterator-profiles PCORE measurement-type slm
          set protocols oam ethernet connectivity-fault-management performance-monitoring sla-iterator-profiles PCORE measurement-interval 5
          set protocols oam ethernet connectivity-fault-management maintenance-domain PCORE level 0
          set protocols oam ethernet connectivity-fault-management maintenance-domain PCORE maintenance-association PCORE continuity-check interval 100ms
          set protocols oam ethernet connectivity-fault-management maintenance-domain PCORE maintenance-association PCORE mep 100 interface ge-0/3/9
          set protocols oam ethernet connectivity-fault-management maintenance-domain PCORE maintenance-association PCORE mep 100 remote-mep 101 sla-iterator-profile PCORE
        

### Rule name: check-l2svc-l2ckt-conn-status 
        	> Description: "Monitors CFM average and max twoway, forward and backward delay to configured destination "
        	> Synopsis: "CFM delay statistics KPI"
        	> Rule file name: check-l2svc-l2ckt-conn-status.rule
        	> Sensor type: sensor vpls-status-sensor 
        	> Supported HealthBot version: 4.0.0
        	> Supported product:MX, Platforms:A, Junos:18.1R1


        	> Helper files: l2svc-conn-status.yml;
        	> More details:
              Monitors L2Circuit connection status and notify anomalies

### Rule name: check-l2svc-vpls-conn-status 
        	> Description: "Monitors CFM average and max twoway, forward and backward delay to configured destination "
        	> Synopsis: "CFM delay statistics KPI"
        	> Rule file name: check-l2svc-vpls-conn-status.rule
        	> Sensor type: sensor vpls-status-sensor 
        	> Supported HealthBot version: 4.0.0
        	> Supported product:MX, Platforms:A, Junos:18.1R1


        	> Helper files: l2svc-conn-status.yml;
        	> More details:
              Monitors VPLS service connection status and notify anomalies

