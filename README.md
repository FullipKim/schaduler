# schaduler
웹서버 구축하고 DB랑 연동한 스케줄러 시스템


#자바스크립트를 사용해 이벤트를 구현했다.

$(document).ready(function() {
    $("#calendar").fullCalendar({
      events : '{{url_for('scheduler')}}',
      dayClick : function(date){
          $('#start, #end').datepicker('setDate',date._d);
          $('.event_selected').removeClass('event_selected');
          $(this).addClass('event_selected');
      },
      eventClick : function(event){
        $('#id').val(event.id);
        $('#title').val(event.title);
        $('#start').val(event.start.toISOString());
        $('#end').val(event.end == null ? event.start.toISOString() : event.end.toISOString() );
        $('.event_selected').removeClass('event_selected');
        $(this).addClass('event_selected');
     }
   });
   
   #schedulerdao.py를 통해 스케쥴러 데이터베이스를 처리하였다.
   #넘어온 schedule을 등록
def setScheduler(schedule):
    #넘어온 데이터중 빈값이 있으면 0 리턴
    if not parameter_checker(schedule) :
        return json.dumps({'rows' : 0})
    else :
        sql = "INSERT INTO my_schedule(title, start, end, allDay) VALUES (%s, %s, %s, %s)"
        params = (schedule['title'], schedule['start'], schedule['end'], schedule['allDay'])
        return json.dumps({'rows' : sql_template(3, sql, params)})

# schedule 삭제
def delScheduler(id):
    #넘어온 데이터중 빈값이 있으면 0 리턴
    if not parameter_checker(id) :
        return json.dumps({'rows' : 0})
    else :
        sql = "DELETE FROM my_schedule WHERE id = %s"
        params = (id)
        return json.dumps({'rows' : sql_template(3, sql, params)})

#넘어온 schedule id에 해당하는 schedule을 수정
def putScheduler(schedule):
    #넘어온 데이터중 빈값이 있으면 0 리턴
    if not parameter_checker(schedule) :
        return json.dumps({'rows' : 0})
    else :
        sql = "UPDATE my_schedule SET title = %s, start = %s, end = %s, allDay = %s WHERE id = %s"
        params = (schedule['title'], schedule['start'], schedule['end'], schedule['allDay'], schedule['id'])
        return json.dumps({'rows' : sql_template(3, sql, params)})
