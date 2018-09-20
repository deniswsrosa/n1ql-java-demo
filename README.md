# n1ql-java-demo


    @Override
    public List<MaintenanceSchedule> listSchedules(String companyId, String areaId, String checklistId, String familyId,
                                                   Long targetDateTime ) {
        String queryString = "Select meta(t).id as id, t.* from " + getBucketName() + " t where t._class = '" + MaintenanceSchedule.class.getName() + "'"
                + " and t.companyId = '" + companyId + "' and t.areaId = '"+areaId+"' and t.checklistId = '"+checklistId+"' "+
                " and t.familyId = '"+familyId+"' and t.removed = false and t.targetDateTime = "+targetDateTime;
                
        N1qlParams params = N1qlParams.build().consistency(ScanConsistency.REQUEST_PLUS).adhoc(true);
        ParameterizedN1qlQuery query = N1qlQuery.parameterized(queryString, JsonObject.create(), params);
        return maintenanceScheduleRepository.getCouchbaseOperations().findByN1QLProjection(query, MaintenanceSchedule.class);
    }
